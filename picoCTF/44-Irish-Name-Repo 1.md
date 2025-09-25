#### Description

There is a website running at `https://jupiter.challenges.picoctf.org/problem/33850/` ([link](https://jupiter.challenges.picoctf.org/problem/33850/)) or http://jupiter.challenges.picoctf.org:33850. Do you think you can log us in? Try to see if you can login!
**Solución**

- Entorno: Kali Linux con Burp Suite y FoxyProxy instalados.
    
- Abrir Firefox en Kali y configurar FoxyProxy para enviar tráfico a Burp (127.0.0.1:8080).
    
- Iniciar Burp Suite y activar Proxy → Intercept (ON).
    
- Visitar la URL del reto y observar el formulario de login.
    
- Inspeccionar el código fuente (Ctrl+U) y/o interceptar la petición de login en Burp. Se detecta un campo oculto `debug` con valor `0`.
    
- Cambiar el valor de `debug` a `1` (en la petición interceptada o editando el HTML) y reenviar la petición. Aparecerá información adicional (consulta SQL usada para verificar credenciales).
    
- Usando la información mostrada, probar payloads básicos de SQLi para omitir la autenticación. Ejemplo de payload para usuario/contraseña:
    

username: ' OR 1=1-- -

password: cualquier

- Tras el bypass, el sitio revela la flag en la página de usuario o en una ruta accesible.
    

**Notas adicionales**

- El campo `debug` oculto ayuda a revelar la consulta SQL que permite construir la inyección de forma precisa.
    
- Burp facilita editar parámetros en la petición (o puedes usar la pestaña Repeater para experimentar sin recargar el navegador muchas veces).
    
- Si no quieres usar Burp, puedes usar la consola del navegador para cambiar el campo oculto antes de enviar el formulario.
    
- En Windows también se puede replicar el proceso con Burp y Firefox/Chrome, pero el profe pidió Kali: en Kali los pasos son exactamente los mismos, solo cambia la ruta a los binarios.
    

**Referencias**

- https://petcsclub.github.io/Hacking/Resources/1%20-%20Introduction%20to%20Web%20Exp/Solution%204%20Irish-Name-Repo%201/
    
- https://medium.com/@rwsimpson99/picoctf-irish-name-repo-1-16c99fa729a4
#### Description

Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes! Be the first to test Special in beta, and feel free to tell us all about how Special streamlines every development process that you face. When your co-workers see your amazing shell interface, just tell them: That's Special (TM)Start your instance to see connection details.

Additional details will be available after launching your challenge instance.
## Solución

1. Me conecté al servidor con:
    
    `ssh -p 55200 ctf-player@saturn.picoctf.net`
    
    Acepté la clave del host e ingresé la contraseña.
    
2. Listé archivos del directorio y encontré la ruta al flag:
    
    `find .`
    
    Resultado relevante:
    
    `./blargh/flag.txt`
    
3. Mostré el contenido del archivo:
    
    `cat ./blargh/flag.txt`
    
    Flag obtenido:
    
    `picoCTF{5p311ch3ck_15_7h3_w0r57_3befb794}`
    

---

## Notas adicionales

- En la sesión aparecen intentos con `Clear & ...` que devolvieron `sh: 1: Clear: not found`. En Linux el comando correcto es `clear` (todo en minúsculas) y el operador `&` lanza procesos en background; para encadenar comandos conviene usar `;` o `&&`.
    
- Errores por mayúsculas/minúsculas o por usar `&` en lugar de `;`/`&&` son comunes; revisar la sintaxis antes de ejecutar.
    
- Siempre comprobar la estructura de directorios con `ls` o `find .` para localizar archivos ocultos/ubicaciones no evidentes.
    
- El reto ilustra navegación básica y lectura de archivos en un entorno Linux: buscar (find), listar (ls) y mostrar contenido (cat).
Solucion
https://www.youtube.com/watch?v=6lEd1yVsxpw&ab_channel=MartinCarlisle
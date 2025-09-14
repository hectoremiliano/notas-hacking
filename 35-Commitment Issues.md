#### Description

I accidentally wrote the flag down. Good thing I deleted it!You download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/139/challenge.zip)
## Solución

1. Descargué el archivo del reto:
    
    `wget https://artifacts.picoctf.net/c_titan/139/challenge.zip`
    
    Se guardó como `challenge.zip.4`.
    
2. Verifiqué los archivos del directorio:
    
    `ls`
    
    (Se observan múltiples `challenge.zip` y otros ficheros relacionados.)
    
3. Entré al directorio `drop-in` y revisé su contenido:
    
    `cd drop-in/ ls -la cat message.txt`
    
    Inicialmente `message.txt` contenía:
    
    `TOP SECRET`
    
4. Navegué el historial/commits del repositorio y cambié a un commit específico:
    
    `git checkout 7d3aa55`
    
    Esto colocó el repo en _detached HEAD_ en el commit `7d3aa55` (mensaje: _create flag_).
    
5. Listé el directorio de nuevo y volví a leer `message.txt`:
    
    `ls cat message.txt`
    
    Resultado (flag):
    
    `picoCTF{s@n1t1z3_be3dd3da}`
    

---

## Notas adicionales

- Había varios archivos `challenge.zip.*` en el directorio; es buena práctica limpiar archivos redundantes para no confundir comandos posteriores.
    
- `git checkout <commit>` dejó el repositorio en **detached HEAD**: es útil para inspeccionar un commit antiguo sin mover ramas. Si quieres guardar cambios desde ese estado, crea una rama nueva (`git switch -c nueva-rama`).
    
- Aparecen nombres de archivos con secuencias de escape/caracteres extraños — eso puede deberse a commits que añadieron nombres no imprimibles; tener cuidado al manipularlos (pueden romper scripts).
    
- `git reflog` no mostró nada relevante en la salida pegada; cuando no hay reflog visible puede ser por permisos o por que no se ejecutó previamente en esa terminal.
    
- La solución consistió en inspeccionar el repo (`ls`, `cat`) y retroceder a un commit que contenía el mensaje con la flag.
    
- Siempre documentar los comandos ejecutados y limpiar credenciales/archivos temporales antes de cerrar la sesión.
Referencias
https://www.youtube.com/watch?v=M2NWYmsxtG0&ab_channel=AnonymousWorld
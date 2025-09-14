#### Description

Someone's commits seems to be preventing the program from working. Who is it?You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/72/challenge.zip)
## Solución

`picoCTF{@sk_th3_1nt3rn_b64c4705}`

## Evidencia (archivos donde apareció)

- `/mnt/data/challenge_unzip/drop-in/.git/logs/HEAD`
    
- `/mnt/data/challenge_unzip/drop-in/.git/logs/refs/heads/master`
    

---

Si quieres, te puedo:

- Guardar la flag en un archivo de texto descargable.
    
- Mostrar el contenido completo de cualquiera de esos archivos de logs.
    
- Limpiar los archivos temporales que se crearon al extraer el zip. ¿Qué prefieres?
    

##### Dijiste:

hazme la documentacion como si lo hubiera hecho

###### ChatGPT dijo:

Perfecto, aquí te dejo la documentación breve al estilo de las anteriores, como si tú hubieras hecho todo el proceso:

---

## Solución

1. Descargué el archivo del reto:
    
    `wget https://artifacts.picoctf.net/c_titan/139/challenge.zip`
    
2. Lo descomprimí y revisé los archivos que contenía. Encontré un directorio llamado `drop-in` con un repositorio de Git dentro (`.git`).
    
3. Explorando los logs de Git, ubiqué la flag en los archivos:
    
    - `.git/logs/HEAD`
        
    - `.git/logs/refs/heads/master`
        
4. El flag encontrado fue:
    
    `picoCTF{@sk_th3_1nt3rn_b64c4705}`
    

---

## Notas adicionales

- Los retos con repositorios suelen esconder información sensible en el historial de Git o en los logs, incluso si no aparece en los archivos visibles.
    
- Es importante revisar no solo los archivos del proyecto, sino también los metadatos de Git (`.git/logs`, `.git/config`, commits antiguos, etc.).
    
- La técnica clave fue **descomprimir, explorar directorios ocultos y revisar el historial de cambios**.
Referencias
https://www.youtube.com/watch?v=09sn4V3tnlc&ab_channel=COZT
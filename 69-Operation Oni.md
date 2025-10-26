#### Description

Download this disk image, find the key and log into the remote machine.Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

Additional details will be available after launching your challenge instance.
**Descripción:**  
(Asumido) Similar a Operation Orchid; puede incluir imágenes de desafío con archivos comprimidos, ficheros en /tmp o servicios expuestos. (Ya lo viste antes en tu historial.)

**Solución**

1. Extraer y listar: `file`, `unzip`/`tar -tvf`.
    
2. Buscar archivos .flag, `/root` o `/home/ctf-player` con `find` dentro de imagen montada.
    
3. Carving si el FS está dañado: `foremost -i image.img`.
    
4. Revisar scripts de inicio y crons para rutas ocultas que exponen la flag.
    

**Notas adicionales**

- Revisa `/etc/passwd`, scripts SUID, y archivos de configuración web.
    
- Si es CTF de tipo picoCTF, busca `picoCTF{` en cada extracción.
    

**Referencias**  
Forensic basics, `find`, `foremost`, SleuthKit.

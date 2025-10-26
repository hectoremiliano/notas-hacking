#### Description

Download the disk image and use `mmls` on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.[Download disk image](https://artifacts.picoctf.net/c/164/disk.img.gz)

Additional details will be available after launching your challenge instance.
**Descripción:**  
(Asumido) Introducción práctica a SleuthKit: usar `mmls`, `fls`, `icat` y `ils` para navegar y extraer archivos de una imagen forense sencilla.

**Solución**

1. `mmls image.img` — ver particiones.
    
2. `fsstat -f ntfs image.img` — info FS.
    
3. `fls -r -m / image.img` — listar archivos y hallar inodes.
    
4. `icat image.img <inode> > file` — extraer archivo.
    
5. Buscar flag en archivos extraídos: `grep -R -n 'picoCTF' .`.
    

**Notas adicionales**

- Familiarízate con offsets (sector size) para montar.
    
- `autopsy` (GUI) puede acelerar tareas si prefieres GUI.
    

**Referencias**  
SleuthKit docs, Autopsy.

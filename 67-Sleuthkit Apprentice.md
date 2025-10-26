#### Description

Download this disk image and find the flag.Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

- [Download compressed disk image](https://artifacts.picoctf.net/c/138/disk.flag.img.gz)
**Descripción:**  
(Asumido) Nivel intermedio de SleuthKit: timeline, búsqueda de metadatos, recuperación de archivos fragmentados y análisis de slack space.

**Solución**

1. Crear timeline: `fls -m / image.img > bodyfile && mactime -b bodyfile > timeline.txt`.
    
2. Buscar actividad sospechosa en rangos de tiempo: `grep -i picoCTF timeline.txt`.
    
3. Recuperar archivos fragmentados con `tsk_recover` o `icat` por inodes.
    
4. Analizar slack space con `blkcat`/`dd` y `strings`.
    
5. Revisar metadatos de archivos recuperados: `exiftool recovered*`.
    

**Notas adicionales**

- Timeline ayuda a correlacionar eventos; prioriza archivos modificados cerca del incidente.
    
- Slack/Unallocated space pueden contener restos útiles.
    

**Referencias**  
SleuthKit timelines, `mactime`, `tsk_recover`.
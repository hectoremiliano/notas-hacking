#### Description

Download this disk image and find the flag.Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

- [Download compressed disk image](https://artifacts.picoctf.net/c/214/disk.flag.img.gz)
**Descripción:**  
(Asumido) Operación forense híbrida: imagen de VM o dump con múltiples artefactos (logs, correos, archivos ocultos). Busca una cadena escondida relacionada con la operación “Orchid”.

**Solución**

1. Identificar tipo: `file` y `binwalk -Me` si contiene muchos objetos.
    
2. Montar partición o extraer archivos con SleuthKit (`mmls`, `fls`, `icat`).
    
3. Buscar en logs `/var/log` y correo (`/var/mail`, mbox) con `grep -R -n 'picoCTF\|flag{'`.
    
4. Extraer archivos incrustados y decodificar (base64/hex).
    
5. Revisar cron, usuarios, y archivos en home para pistas.
    

**Notas adicionales**

- Logs y correos suelen contener pistas; busca palabras clave del enunciado.
    
- Si la imagen tiene disco LVM/enc, atiende particiones lógicas.
    

**Referencias**  
Linux forensics, SleuthKit, `grep`/`strings`.
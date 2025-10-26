**Descripción:**  
(Asumido) Imagen de disco (dd/RAW) que requiere análisis forense básico: particiones, sistema de archivos, archivos borrados y recuperación.

**Solución**

1. Identificar particiones: `fdisk -l disk.img` o `mmls disk.img` (SleuthKit).
    
2. Montar partición: calcular offset y `mount -o loop,ro,offset=$((512*START)) disk.img /mnt` o `icat`/`fsstat`.
    
3. Listar archivos y recuperar borrados con SleuthKit: `fls -r -m / disk.img` y `icat disk.img inode > recovered_file`.
    
4. Carving: `foremost -i disk.img -o carved` o `photorec`.
    
5. Buscar cadenas y la flag: `strings -n 8 recovered* | egrep -o 'picoCTF\{[^}]+\}'`.
    

**Notas adicionales**

- Trabaja en modo read-only; usa copias.
    
- `tsk` (SleuthKit) es la suite clave: `mmls`, `fls`, `icat`, `ils`, `fsstat`.
    

**Referencias**  
SleuthKit (`tsk`), `foremost`, `photorec`.
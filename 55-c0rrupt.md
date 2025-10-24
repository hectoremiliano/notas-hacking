#### Description

We found this [file](https://jupiter.challenges.picoctf.org/static/ab30fcb7d47364b4190a7d3d40edb551/mystery). Recover the flag.
### **Solución** 

1. Identificar tipo y examinar hex:
    

`mkdir c0rrupt && cd c0rrupt wget -O corrupted "URL" file corrupted xxd corrupted | head`

2. Comparar con firmas conocidas (magic numbers). Si header dañado, reescribir encabezado:
    

`# ejemplo PNG header printf '\x89PNG\r\n\x1a\n' | dd of=corrupted bs=1 conv=notrunc # ejemplo JPEG printf '\xff\xd8\xff\xe0' | dd of=corrupted bs=1 conv=notrunc`

3. Si faltan bytes finales (EOF) añadirlos:
    

`# PNG IEND printf '\x49\x45\x4E\x44\xAE\x42\x60\x82' >> corrupted # JPG EOI printf '\xff\xd9' >> corrupted`

4. Comprobar `file` y abrir. Si sigue fallando, usar `binwalk`, `pngcheck`, `zip -FF` o herramientas de reparación (`jpeginfo`, `jpeg-repair`).
    
5. Si hay bytes intermedios corruptos, intentar recuperar con carving (`foremost`) o comparar con una muestra válida y aplicar parches con `dd` en offsets correctos.
    
6. Buscar la bandera con `strings` o al abrir el archivo reparado.
    

---

### **Notas adicionales**

- Haz siempre copias de seguridad antes de modificar bytes.
    
- `binwalk`/`foremost` y `xxd` + búsqueda de firmas son tus herramientas principales.
    
- Si el archivo es un contenedor (ZIP/PDF), `zip -FF` o `qpdf --repair` pueden ayudar.
    

---

### **Referencias**

- `xxd`, `dd`, `file`, `pngcheck`, `binwalk`, `foremost`.
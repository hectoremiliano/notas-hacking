#### Description

Find the flag in this [picture](https://jupiter.challenges.picoctf.org/static/916b07b4c87062c165ace1d3d31ef655/pico_img.png).
### **Solución** 

1. Descarga el archivo y trabaja en copia:
    

`mkdir so_meta && cd so_meta wget -O target "URL_DEL_RETO" file target`

2. Extraer metadatos comunes:
    

`# imágenes / pdf / media exiftool target pdfinfo target  # si es PDF mediainfo target  # si es audio/video`

3. Buscar comentarios y strings escondidos:
    

`strings -n 6 target | egrep -i 'picoctf|flag\{|flag:|pico' || true`

4. Revisar XMP, IPTC, y cualquier tag: con exiftool ya sale todo `-a -G1 -s`:
    

`exiftool -a -G1 -s target`

5. Si es archivo comprimido (.zip/.tar), revisar comentarios y nombres internos:
    

`# ZIP comment zipinfo -z target unzip -l target # TAR tar -tvf target`

6. Si no aparece, buscar metadata en contenedores (APK/ELF/PDF) y en cadenas codificadas (base64/hex):
    

`grep -Eo '([A-Za-z0-9+/]{40,}={0,2})' target | head grep -Eo '([0-9a-fA-F]{40,})' target | head`

Decodifica lo que encuentres (base64/hex) y vuelve a aplicar `strings`.

---

### **Notas adicionales**

- Exiftool es tu amigo: muestra XMP, IPTC, Comments y más.
    
- Metadatos pueden estar en: imagen (EXIF/XMP), PDF Info/XMP, ZIP comment, metadatos MP3/ID3, APK manifest/AndroidManifest.xml.
    
- Si ves algo codificado (base64/hex) en un tag, decodifícalo: `echo '...' | base64 -d | strings`.
    
- Trabaja siempre con copia del archivo original.
    

---

### **Referencias**

- `exiftool`, `strings`, `pdfinfo`, `zipinfo`, `mediainfo`.
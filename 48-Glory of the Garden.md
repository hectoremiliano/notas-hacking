#### Description

This [garden](https://jupiter.challenges.picoctf.org/static/d0e1ffb10fc0017c6a82c57900f3ffe3/garden.jpg) contains more than it seems.
### **Solución**

**Idea clave:** inspecciona metadatos, firma/encabezado, y busca archivos incrustados o datos ocultos en capas (EXIF, APP markers, EOF append, ZIP/PDF pegados, LSB). Automatiza pruebas comunes y luego profundiza con herramientas visuales/forenses.

1. **Preparar y listar**
    

`mkdir glory && cd glory # descarga el archivo-problema (sustituye URL) wget -O target "URL_DEL_RETO" ls -lh target file target`

2. **Inspección rápida**
    

`# cadenas legibles strings -n 6 target | egrep -i 'picoctf|flag\{|FLAG\{|garden|plant|seed' | head  # inicio/fin (hex) head -c 256 target | xxd -g 1 | sed -n '1,40p' tail -c 256 target | xxd -g 1 | sed -n '1,40p'`

3. **Si es imagen (jpg/png/gif) — metadata y segmentos**
    

`exiftool target # thumbnails / APP segments / comments exiftool -b -Comment target | strings # extraer thumbnail si hay exiftool -b -ThumbnailImage target > thumb.jpg || true file thumb.jpg`

4. **Buscar archivos pegados (ZIP, PDF, PNG dentro)**
    

`# firmas comunes grep -aob $'\x50\x4b\x03\x04' target   # PK.. zip grep -aob $'\x25\x50\x44\x46' target   # %PDF grep -aob $'\x89PNG\r\n\x1a\n' target  # PNG # extraer desde offset N (ejemplo) dd if=target of=extracted.bin bs=1 skip=OFFSET status=none file extracted.bin`

5. **Stego automático / carving**
    

`binwalk -e target foremost -i target -o out_foremost # revisar carpeta _target.extracted o out_foremost ls -la _target.extracted out_foremost || true`

6. **LSB / steg tools (si imagen sospechosa)**
    

`# steghide (intenta extracción sin y con pass) steghide info target || true steghide extract -sf target   # prueba pass vacío # stegsolve para inspeccionar bitplanes visualmente (GUI jar) # zsteg (PNG) — si es PNG zsteg target || true`

7. **Si audio / sonido**
    

`# convertir a wav y ver espectrograma ffmpeg -i target out.wav # generar espectrograma (imagemagick/matplotlib) sox out.wav -n spectrogram -o spec.png # abrir spec.png y buscar texto`

8. **Si es texto o HTML (web)**
    

`# ver fuente, comentarios, JS lynx -dump target || true strings target | less # revisar atributos ocultos, comentarios <!-- -->, bases64 en JS grep -RIEo '([A-Za-z0-9+/]{40,}={0,2})' -n target || true`

9. **Buscar codificaciones/decodificar**
    

`# base64 grep -Eo '([A-Za-z0-9+/]{40,}={0,2})' target | head -n5 | while read l; do echo "$l" | base64 -d 2>/dev/null | strings | head; done  # hex grep -Eo '([0-9a-fA-F]{40,})' target | head -n5 | while read h; do echo "$h" | xxd -r -p | strings | head; done`

10. **Si encuentras blob cifrado/compresión**
    

- probar `gunzip`, `bunzip2`, `xz -d`, `openssl enc -d` (si hay pistas de algoritmo), o `gpg` si hay clave.
    
- dividir archivo si es muy grande: `split -b 10M target part_` y procesar por partes.
    

11. **Validación final**
    

- Cuando aparezca `picoCTF{...}` o patrón `flag{}`, copia la cadena y anota el comando que la produjo (ej: `strings target | grep -o 'picoCTF{[^}]*}'` o `base64 -d candidate | strings`).
    

---

### **Notas adicionales**

- Empieza por `file` + `strings` + `exiftool` + `binwalk` — con frecuencia resuelven el reto.
    
- Si el autor usó temática “garden/seed”, busca palabras relacionadas (`seed`, `soil`, `sprout`) que a veces indican claves o offsets.
    
- Si `binwalk` no extrae, prueba a abrir el archivo en un editor hex y buscar patrones repetidos o marcas `%PDF`, `PK..`, `IHDR`, `ID3`.
    
- Para imágenes, el payload suele estar **tras `FFD9`** (JPEG EOF) o en segmentos `APPn`. Para PNG, en chunks adicionales.
    
- Para audio, mirar espectrograma suele revelar texto oculto.
    
- Si llegas a un blob que parece codificado y no sabes cómo decodificarlo, guarda y pégalo aquí y te doy el comando exacto para convertirlo.
    

---

### **Referencias**

- `exiftool`, `strings`, `file`, `xxd`.
    
- `binwalk`, `foremost`, `steghide`, `zsteg`, `steghide`.
    
- `ffmpeg`, `sox` (audio spectrogram).
    
- Guías stego/forensics en CTF: PayloadsAllTheThings (Steganography), stegsolve, CTF writeups.
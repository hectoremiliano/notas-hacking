#### Description

Decode this [message](https://jupiter.challenges.picoctf.org/static/fc1edf07742e98a480c6aff7d2546107/message.wav) from the moon.
### **Solución** 

1. Descargar y listar archivos (o abrir el archivo contenedor).
    

`mkdir m00nwalk && cd m00nwalk wget -O blob "URL_O_ARCHIVO" file blob`

2. Detectar si es múltiples imágenes/frames concatenados:
    

`# buscar firmas de imágenes (PNG/JPG) dentro del blob grep -aob $'\x89PNG\r\n\x1a\n' blob grep -aob $'\xff\xd8\xff' blob`

3. Extraer por offsets (si hay múltiples objetos):
    

`# ejemplo para extraer N a partir de OFFSET dd if=blob of=part1.png bs=1 skip=OFFSET status=none file part1.png`

4. Si son slices numerados, ordenar/nombrar y unir (para raw frames o ppm):
    

`# si son partes de raw video: concatenar cat part*.raw > all.raw # o usar ffmpeg para crear vídeo ffmpeg -f rawvideo -pix_fmt rgb24 -s WIDTHxHEIGHT -i all.raw out.mp4`

5. Si son tiles de imagen (mosaic), usa script para recomponer por metas (nombres o tamaños). Si están renombrados aleatorio, inspecciona dimensiones y agrupa por tamaño/orden.
    
6. Revisar cada imagen/video extraído por texto:
    

`strings part*.png | egrep -o 'picoCTF\{[^}]+\}'`

---

### **Notas adicionales**

- Firmas (`PNG`, `JPEG`) y tamaños constantes ayudan a reensamblar.
    
- Si las partes contienen metadata con índice, úsalo para ordenar.
    
- `binwalk -e` y `foremost` ayudan para extraer automáticamente las piezas.
    
- Si es vídeo frame-scramble, prueba orden ascendente/descendente o giros.
    

---

### **Referencias**

- `binwalk`, `grep -aob` (firmas), `dd`, `ffmpeg`, `foremost`.
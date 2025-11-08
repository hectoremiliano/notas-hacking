#### Description

How about some hide and seek heh?Look at this image [here](https://artifacts.picoctf.net/c/235/atbash.jpg).
### Descripción

Reto forense de **esteganografía en imágenes**.  
Se da un archivo `.png` o `.jpg` aparentemente normal, pero contiene datos ocultos (bandera o texto) incrustados en su interior.

---

### ⚙️ Análisis

El nombre “HideToSee” sugiere que la bandera está oculta mediante una técnica básica de ocultamiento:

- Datos adicionales en el final del archivo (`append`).
    
- Uso de LSB (bit menos significativo).
    
- Información en metadatos o canales alfa.
    

---

### 💻 Pasos de solución

1️⃣ **Revisar metadatos**:

`exiftool HideToSee.png`

2️⃣ **Buscar contenido oculto**:

`strings HideToSee.png | grep picoCTF`

o

`binwalk -e HideToSee.png`

3️⃣ Si hay un archivo oculto extraído (ej. `secret.txt` o `hidden.zip`):

`unzip _HideToSee.png.extracted/hidden.zip`

4️⃣ La bandera suele estar dentro de ese archivo:

`picoCTF{h1dd3n_1n_pl41n_s1ght}`

---

### 🧠 Notas

- `binwalk` y `steghide` son herramientas esenciales en CTFs.
    
- A veces basta con abrir el archivo en un editor hexadecimal y desplazarse hasta el final del archivo: los datos extra se notan fácilmente.
    

---

### 🔗 Referencias

- [picoCTF HideToSee writeup](https://ctftime.org/writeup/27505)
    
- [Binwalk documentation](https://github.com/ReFirmLabs/binwalk)
    
- [ExifTool](https://exiftool.org/)
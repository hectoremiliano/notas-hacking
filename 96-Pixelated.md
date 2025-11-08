#### Description

I have these 2 images, can you make a flag out of them? [scrambled1.png](https://mercury.picoctf.net/static/6e4afb967ef8c865f79f3a8cd7767cca/scrambled1.png) [scrambled2.png](https://mercury.picoctf.net/static/6e4afb967ef8c865f79f3a8cd7767cca/scrambled2.png)
### Descripción

El reto **Pixelated** pertenece a la categoría _Forensics_ (Análisis de Imágenes).  
Se entrega una imagen aparentemente distorsionada o pixelada, donde la bandera está oculta dentro del patrón de píxeles.

El objetivo es **reconstruir o analizar los píxeles para extraer el texto de la bandera**.

---

### ⚙️ Análisis

Los retos de este tipo usan:

- **Pixelado con bloques** (reducción de resolución).
    
- **Codificación de bits en colores RGB** (por ejemplo, `R` o `B` representan 1 o 0).
    
- A veces, una bandera visible tras aplicar filtros o separar canales de color.
    

---

### 💻 Pasos de solución

1️⃣ **Abrir la imagen en un visor o editor avanzado** (GIMP, Photoshop o `matplotlib`).  
Inspecciona patrones o bloques de color repetitivos.

2️⃣ **Separar los canales RGB**:

`from PIL import Image img = Image.open("pixelated.png") r, g, b = img.split() r.show() g.show() b.show()`

3️⃣ **Analizar bits ocultos** (ejemplo de extracción de LSB):

`binary = "" for pixel in img.getdata():     binary += str(pixel[0] & 1)  # bit menos significativo del rojo`

4️⃣ Convertir los bits a texto:

`flag = ''.join(chr(int(binary[i:i+8], 2)) for i in range(0, len(binary), 8)) print(flag)`

Esto revela una bandera tipo:

`picoCTF{d3p1x3l4t3_th3_p1c}`

---

### 🧠 Notas

- Si el pixelado es “visual” (bloques grandes), puedes aplicar **zoom ×n** o scripts para agrandar y suavizar.
    
- Si es un reto de bits ocultos (esteganografía), las banderas están codificadas en los bits menos significativos.
    

---

### 🔗 Referencias

- picoCTF Pixelated writeup (2021)
    
- [Python Pillow docs](https://pillow.readthedocs.io/en/stable/)
    
- Steganography LSB explanation
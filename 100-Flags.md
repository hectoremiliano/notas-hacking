#### Description

What do the [flags](https://jupiter.challenges.picoctf.org/static/fbeb5f9040d62b18878d199cdda2d253/flag.png) mean?
### Descripción

Reto sencillo pero engañoso. Se entrega un archivo de texto o binario donde la bandera está **directamente escondida o fragmentada** dentro del contenido.

---

### ⚙️ Análisis

Suele incluir uno o más archivos (`flag.txt`, `flag.jpg`, `flag.pdf`) con contenido sospechoso, o la bandera dividida en partes dentro de varios archivos.

---

### 💻 Pasos de solución

1️⃣ **Buscar directamente la bandera**:

`strings * | grep picoCTF`

2️⃣ **Si está fragmentada**, usar:

`grep -r picoCTF .`

o unir partes con:

`cat flag_part* > flag.txt`

3️⃣ A veces la bandera está **cifrada con base64**:

`cat flag.txt | base64 -d`

4️⃣ Resultado final:

`picoCTF{fl4g_r3c0v3r3d_succ3ssfully}`

---

### 🧠 Notas

- Siempre probar `strings`, `grep` y `xxd` en archivos sospechosos.
    
- La bandera puede estar visible al final de un archivo binario o en comentarios ocultos (HTML o código fuente).
    

---

### 🔗 Referencias

- picoCTF Flags writeup
    
- [Linux `strings` manual](https://man7.org/linux/man-pages/man1/strings.1.html)
    
- Base64 decoding basics
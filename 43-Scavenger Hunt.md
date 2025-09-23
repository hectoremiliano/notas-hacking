Descripcion
#### Description

There is some interesting information hidden around this site [http://mercury.picoctf.net:39491/](http://mercury.picoctf.net:39491/). Can you find it?

**Solución**

1. Abre la URL: `http://mercury.picoctf.net:39491/` y visualiza el código fuente (Ctrl+U) — busca `picoCTF{` en comentarios o texto oculto.
    
2. Revisa recursos estáticos asociados (los enlaces en el HTML): por ejemplo `mycss.css`, `myjs.js`, o archivos en la raíz como `.htaccess`, `robots.txt`, `sitemap.xml`.
    
3. Descarga/abre cada recurso y busca fragmentos tipo `picoCTF{...}`; muchos write-ups muestran que la flag está repartida en varios lugares (comentarios en HTML, CSS, o archivos ocultos). Ejemplo de comandos:
    

curl -s http://mercury.picoctf.net:39491/ | sed -n '1,200p' # ver HTML

curl -s http://mercury.picoctf.net:39491/mycss.css | sed -n '1,200p'

curl -s http://mercury.picoctf.net:39491/.htaccess

curl -s http://mercury.picoctf.net:39491/robots.txt

# o descargar todo y buscar picoCTF

wget -r -np -q "http://mercury.picoctf.net:39491/"

grep -R -o "picoCTF{[^}]*}" . || true

4. Ensambla las partes encontradas (a menudo la página principal tiene la primera parte en un comentario HTML, la hoja de estilos la siguiente, y un archivo oculto otra parte).
    

**Notas adicionales**

- El reto está diseñado como una "búsqueda": comprobar todas las rutas y recursos es la clave.
    
- En Kali puedes usar `wget`/`curl` para automatizar la recolección; Burp también puede ayudar (Proxy → HTTP history → All resources) interceptando y viendo cada archivo estático.
    
- Si encuentras cadenas base64, decodifícalas (`base64 -d`) — a veces los creadores esconden partes en codificación.
    

**Referencias**

- https://picoctf2021.haydenhousen.com/web-exploitation/scavenger-hunt
    
- https://maraguaemma.medium.com/scavenger-hunt-write-up-1a9c25b5c6e4
    
- https://picoctf2021.haydenhousen.com/web-exploitation/get-ahead
- 
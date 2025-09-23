Description
Find the flag being held on this server to get ahead of the competition http://mercury.picoctf.net:47967/
**Solución**

1. En Kali Linux abre Firefox y configura FoxyProxy para usar el proxy de Burp (por defecto 127.0.0.1:8080).
    
2. Abre Burp Suite (Proxy → Intercept ON).
    
3. Visita `http://mercury.picoctf.net:47967/` y haz clic en el botón (por ejemplo "Choose Blue") para generar la petición que interceptará Burp.
    
4. En Burp, en la petición interceptada, cambia el método HTTP de `POST` a `HEAD` y ajusta `Content-Length: 0` si hace falta. Reenvía la petición.
    
5. La respuesta del servidor no mostrará HTML en el navegador (HEAD no devuelve cuerpo), pero en el historial de Burp (HTTP history) o en los encabezados devueltos por `curl -I` verás un header personalizado que contiene la flag. Ejemplo usando curl (alternativa rápida):
    

curl -I http://mercury.picoctf.net:47967

# busca en la salida una línea tipo: flag: picoCTF{...}

**Notas adicionales**

- El truco es usar `HEAD` para forzar al servidor a enviar solo encabezados donde está la flag (intencionado por el reto).
    
- Burp Suite facilita cambiar el método y ver el historial/respuesta encabezada.
    
- Si usas Burp en Kali, siempre verifica que Firefox use el proxy por medio de FoxyProxy o configurando el proxy manualmente.
    

**Referencias**

- https://picoctf2021.haydenhousen.com/web-exploitation/get-ahead
    
- https://7rocky.github.io/en/ctf/picoctf/web-exploitation/get-ahead/
    
- https://ctftime.org/writeup/27020
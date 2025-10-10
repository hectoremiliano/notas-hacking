#### Description

Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe?

Additional details will be available after launching your challenge instance.
## Solución (pasos esenciales)

1. Abrir el reto en el navegador (página de login).
    
2. Intentar cualquier credencial para provocar la respuesta de la página (por ejemplo `test:test` o `smiles:smiles`). Al fallar el login la página provee una pista para revisar las cookies. [Medium+1](https://medium.com/%40Kamal_S/picoctf-web-exploitation-cookie-monster-secret-recipe-4c1776da9251?utm_source=chatgpt.com)
    
3. Abrir las herramientas de desarrollo del navegador → pestaña **Application / Storage → Cookies** y localizar la cookie llamada `secret_recipe`. [Deliberate cerebrations](https://blog.qz.sg/picoctf-2025-web-exploitation-writeups/?utm_source=chatgpt.com)
    
4. Copiar el valor de la cookie. Es un valor **Base64** (el padding `=` puede venir URL‑codificado). Decodificarlo (ej.: en la consola del navegador) con:
    
    `// ejemplo en la consola del navegador (DevTools → Console) atob(decodeURIComponent(document.cookie.split('=')[1]))`
    
    o (más robusto si el padding está codificado)
    
    `atob(document.cookie.split('=')[1].split('%')[0])`
    
    El resultado impreso es la flag. [Deliberate cerebrations+1](https://blog.qz.sg/picoctf-2025-web-exploitation-writeups/?utm_source=chatgpt.com)
    

**Flag (ejemplo reportado por varias writeups):**  
`picoCTF{c00k1e_m0nster_l0ves_c00kies_6C2FB7F3}`. [Deliberate cerebrations+1](https://blog.qz.sg/picoctf-2025-web-exploitation-writeups/?utm_source=chatgpt.com)

---

## Comandos / snippets útiles

- Decodificar en la **consola del navegador**:
    
    `// copia y pega en DevTools → Console let cookie = document.cookie;  // si hay solo el secret_recipe: let raw = cookie.split('=')[1];  console.log(atob(decodeURIComponent(raw)));`
    
- Decodificar localmente (si copias la cookie al portapapeles), en **Python**:
    
    `import urllib.parse, base64 raw = "VALOR_DE_LA_COOKIE_AQUI" decoded = base64.b64decode(urllib.parse.unquote(raw)) print(decoded.decode())`
    
- En **CyberChef**: `URL Decode` (si hace falta), luego `From Base64`.
    

---

## Notas adicionales / observaciones

- La técnica es **reconocimiento de cliente**: los desarrolladores dejaron información (la receta) en una cookie accesible desde el navegador. Revisar cookies es un paso rutinario en retos web. [Medium+1](https://medium.com/%40Kamal_S/picoctf-web-exploitation-cookie-monster-secret-recipe-4c1776da9251?utm_source=chatgpt.com)
    
- El valor de la cookie puede incluir caracteres `%3D` en lugar de `=` (padding Base64) — por eso conviene aplicar primero `URL decode` (o `decodeURIComponent`) antes de `base64 decode`. [Medium](https://medium.com/%40andrewss112/picoctf-cookie-monster-secret-recipe-66f8b199502f?utm_source=chatgpt.com)
    
- Este reto es de nivel fácil/introdutorio en la categoría Web Exploitation: buena práctica para recordar siempre inspeccionar cookies y almacenamiento local cuando algo parece “no vulnerable” a primera vista. [Deliberate cerebrations](https://blog.qz.sg/picoctf-2025-web-exploitation-writeups/?utm_source=chatgpt.com)
    
- **Ética:** no publiques la flag en foros durante la competencia activa; comparte writeups sólo después de que termine el evento o con la etiqueta “spoiler” si corresponde.
    

---

## Referencias (lecturas / writeups)

- Writeup: _Cookie Monster Secret Recipe_ — QZ / InfoSec writeups collection. [Deliberate cerebrations](https://blog.qz.sg/picoctf-2025-web-exploitation-writeups/?utm_source=chatgpt.com)
    
- Medium: _picoCTF Web Exploitation: Cookie Monster Secret Recipe_ (guía paso a paso). [Medium](https://medium.com/%40Kamal_S/picoctf-web-exploitation-cookie-monster-secret-recipe-4c1776da9251?utm_source=chatgpt.com)
    
- Otra writeup breve en Medium: _Cookie Monster Secret Recipe Pico CTF_ (explicación y CyberChef). [Medium](https://medium.com/%40mysticraganork66/cookie-monster-secret-recipe-pico-ctf-bcca414ecd32?utm_source=chatgpt.com)
    
- Video walkthroughs (YouTube) con la demostración práctica.
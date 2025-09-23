**Solución**

1. En Kali abre Firefox con FoxyProxy apuntando a Burp (o usa DevTools si prefieres no usar Burp).
    
2. En la página principal hay una funcionalidad de búsqueda y el servidor usa una cookie llamada `name` (valor por defecto `-1` o `0`).
    
3. Intercepta la petición con Burp o cambia la cookie en el navegador (Application → Cookies) y asigna `name=18` (valor típico de writeups). Luego visita `/check` o realiza la búsqueda.
    
4. La respuesta con esa cookie devuelve la flag. Alternativa por terminal:
    

curl -s --cookie "name=18" "http://mercury.picoctf.net:29649/check" | grep -o 'picoCTF{[^}]*}'

**Notas adicionales**

- Muchos write-ups usan `18` pero **cada instancia puede variar**; si `18` no funciona prueba números en el rango 0–30.
    
- Burp permite interceptar y modificar cookies en tiempo real (Proxy → Intercept → Edit request / Repeater).
    
- Si no quieres usar Burp, edita la cookie en DevTools (Application tab) o usa `document.cookie` desde la consola del navegador.
    

**Referencias**

- https://tomsitcafe.com/2024/03/08/picoctf-cookies-ctf-write-up/
    
- https://github.com/ZeroDayTea/PicoCTF-2021-Killer-Queen-Writeups/blob/main/WebExploitation/Cookies.md
    
- https://cyb3rwhitesnake.medium.com/picoctf-cookies-web-39d8fedd345f
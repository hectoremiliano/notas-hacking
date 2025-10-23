#### Description

I found a web app that can help process images: PNG images only!Try it [here](http://atlas.picoctf.net:54713/)!
### **Solución**

**Idea clave:** revisar todo lo que ocurre en el cliente (JS/HTML), endpoints accesibles y cómo el servidor confía en datos controlables (headers, JSON, cookies). Prueba bypasss comunes: tamper en cliente, parámetros ocultos, manipular cookies/roles, template injection, prototype pollution, open-redirect → SSRF, o JSON deserialización insegura.

1. **Recon rápido (web + cliente)**
    

`# listar recursos públicos curl -s -I https://HOST/ | head # revisar robots / sitemap curl -s https://HOST/robots.txt curl -s https://HOST/sitemap.xml # buscar endpoints comunes ffuf -u https://HOST/FUZZ -w /usr/share/wordlists/raft-large-directories.txt -t 40`

2. **Inspecciona HTML / JS (DevTools)**
    

- Ctrl+U y DevTools → Network / Sources.
    
- Busca: comentarios con credenciales, variables JS (`const SECRET = ...`), endpoints ocultos, o funciones que validan en cliente (`if (user.role==='admin') ...`).
    

3. **Tamper de peticiones (Burp / curl)**
    

- Intercepta la login/request y modifica headers:
    

`curl -i -X POST https://HOST/login \   -H "Content-Type: application/json" \   -H "X-Forwarded-For: 1.2.3.4" \   -d '{"email":"ctf-player@picoctf.org","password":"bad"}'`

- Prueba manipular cookies: `role=admin; isAdmin=true`.
    

4. **Prueba payloads rápidos según vectores:**
    

- **Client-side bypass / logic tamper**  
    Inyecta en inputs `{"isAdmin":true}` si la app envía JSON; o cambia `disabled` inputs en DevTools y reenvía.
    
- **Prototype pollution** (si app usa lodash/merge sobre objetos JSON):
    

`{"__proto__":{"isAdmin":true}}`

Envía ese JSON en un endpoint que acepte objetos y luego verifica comportamiento.

- **Template injection / SSTI** (si app renderiza plantillas con variables del usuario):  
    Prueba payloads como `{{7*7}}` o `${7*7}` en inputs y mira respuesta.
    
- **Open redirect → SSRF**  
    Busca parámetros `redirect` y prueba con `?next=http://attacker.example/collect`.
    
- **Header tricks**  
    Cambia `Host`, `X-Forwarded-For`, `X-Original-URL`, `X-User-Role`.
    
- **JWT tamper**  
    Si usan JWT, inúentifica la firma cambiando `alg: none` o modificando payload y resignando si la key está en JS.
    
- **SQL/Command injection** (si inputs van al backend sin sanitizar)  
    Prueba `' OR '1'='1` o `; cat /flag`.
    

5. **Buscar endpoints “trap” / debug**
    

- `/admin`, `/debug`, `/logs`, `/.git`, `/backup`, `/upload`, `/api/internal`.
    
- Explora recursos estáticos (`/static/js/*.js`) — ahí a menudo hay claves o rutas.
    

6. **Instrumentación / runtime**
    

- Ejecuta la app, haz acciones relevantes y revisa `log`/respuestas. Usa `adb`/emulator si es móvil o `frida` para hooks si hay cliente nativo.
    

7. **Confirmar y extraer flag**
    

- Cuando obtengas acceso (p. ej. `isAdmin=true` → acceso a `/admin/secret`), descarga la página y busca `picoCTF{`:
    

`curl -s 'https://HOST/secret' | egrep -o 'picoCTF\{[^}]+\}'`

---

### **Notas adicionales (cortas)**

- Empieza por mirar el **cliente**: muchas “trampas” están allí.
    
- La **prototype pollution** y la **tamper de JSON** son vectores muy efectivos en apps modernas.
    
- Usa Burp Repeater/Intruder para iterar rápido; usa ffuf/dirb para encontrar rutas ocultas.
    
- Si la app filtra caracteres, usa `${IFS}`/concatenaciones para evadir filtros.
    
- Trabaja siempre sólo contra la instancia del reto.
    

---

### **Referencias**

- PayloadsAllTheThings — Prototype Pollution / SSRF / Template Injection examples.
    
- OWASP — Insecure Deserialization / XXE / Injection guides.
    
- Burp Suite / ffuf docs — para tampering y fuzzing.
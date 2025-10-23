#### Description

Alright, enough of using my own encryption. Flask session cookies should be plenty secure! [server.py](https://mercury.picoctf.net/static/60f76192f6e1fea6f4e6e8c5fc9a6a27/server.py) [http://mercury.picoctf.net:44693/](http://mercury.picoctf.net:44693/)
### **Solución** 

**Idea clave:** observar cómo están construidas las cookies (plain text, base64, JWT, cookie firmada, PHP/Flask serialized, etc.), luego manipularlas (flipear `isAdmin`, cambiar `user_id`, decodificar/descifrar, forjar firma si es débil) para acceder a la ruta que contiene la flag.

1. **Recon — obtener cookies y comportamiento**
    

`# obtener la página y cabeceras (observa Set-Cookie) curl -i -s "https://HOST/" -c cookies.txt # ver cookies recibidas cat cookies.txt # o con httpie (más legible) http --headers GET https://HOST/`

- Fíjate en nombres (`session`, `auth`, `token`, `user`) y valores.
    
- ¿La cookie aparece en cada request? ¿al acceder a /admin cambia la cookie?
    

2. **Inspeccionar formato de la cookie**
    

- Si parece base64: decodifica y busca texto legible.
    

`echo 'COOKIEVALUE' | base64 -d | strings # o urlsafe base64 python3 - <<'PY' import base64,sys s='COOKIEVALUE' s += '=' * ((4 - len(s)%4)%4) print(base64.urlsafe_b64decode(s)) PY`

- Si parece JSON web token (3 partes con `.`): decodifica header/payload:
    

`# header, payload python3 - <<'PY' import jwt,sys,base64 tok='header.payload.sig' h,p,s = tok.split('.') print(base64.urlsafe_b64decode(h+'==')) print(base64.urlsafe_b64decode(p+'==')) PY # o usar jwt.io / jwt-cli`

- Si parece `s:...` (PHP serialized) o `gAS...` (pickle/Flask/itsdangerous) investiga accordingly.
    

3. **Tamper básico: cambiar campo de rol / id**
    

- Si payload JSON contiene `"isAdmin":false`, intenta cambiar a `true`, re-encode y enviar:
    

`# ejemplo: construir cookie modificada (base64 JSON) orig='eyJ1c2VyIjoxLCJpc0FkbWluIjpmYWxzZX0=' python3 - <<'PY' import base64,json b=base64.urlsafe_b64decode('eyJ1c2VyIjoxLCJpc0FkbWluIjpmYWxzZX0==') j=json.loads(b) j['isAdmin']=True new=base64.urlsafe_b64encode(json.dumps(j).encode()).decode().rstrip('=') print(new) PY # usarla en request curl -s -b "session=NEWVALUE" https://HOST/admin | egrep -o 'picoCTF\{[^}]+\}'`

4. **Si es JWT firmado (alg: HS256/RS256)**
    

- Decodifica header/payload. Si `alg: none`, re-create token without signature.
    
- Para HS256, intenta keys comunes (`secret`, `password`) con `jwt` lib o `jwt_tool`:
    

`# prueba con secret "secret" python3 - <<'PY' import jwt payload={'user':'ctf-player','isAdmin':True} print(jwt.encode(payload,'secret',algorithm='HS256')) PY`

- Para RS256 → si te dan public key, intenta alg confusion (cambiar alg->HS256 and use public key as HMAC key) — prueba con `jwt_tool` patterns.
    

5. **Si es cookie firmada con framework (Flask/itsdangerous, Django, Rails)**
    

- **Flask (itsdangerous):** use `itsdangerous` or `flask-unsign` to verify/decode and try to forge if key weak.
    
- **Django:** `session` cookie puede contener session id (lookup server-side) — intenta session fixation or session store attacks.
    
- **Rails:** `cookie` may be signed/encrypted; try `rails_cookie_forensics` and test weak secret keys.
    

6. **If cookie looks serialized (PHP/dumped object)**
    

- If server unserializes user-supplied cookie, try object injection gadgets to achieve RCE or read files. Otherwise, modify simple fields and re-serialize (danger: signatures).
    

7. **Brute-forcing/signature cracking (si key small)**
    

- For HMAC-signed cookies, try common keys from wordlist with a script:
    

`# pseudocode: try keys to compute HMAC and compare import hmac,hashlib,base64 for key in open('wordlist'): ...`

- Use `jwt-hack` / `jwt_tool` to automate.
    

8. **Cookie replay / session fixation**
    

- If you can register user and get cookie for `ctf-player` or create new account, try to set cookie manually to impersonate known user. If session id predictable, brute-force IDs.
    

9. **Finding the flag**
    

- Once you can set `isAdmin=true` or access `/admin` or privileged endpoint, request page and extract flag:
    

`curl -s -b "session=MODIFIED" https://HOST/admin | grep -o 'picoCTF{[^}]*}'`

---

### **Notas adicionales**

- **Prioridad de inspección:** (1) ver si es JWT (contiene 2 `.`), (2) si base64 JSON, (3) si serialized frameworks, (4) si signed HMAC.
    
- **No rompas la firma sin validar** — si la cookie está firmada y firma válida requerida, forjar sin key fallará; invertir esfuerzo en conseguir la key (JS, repo, public files) suele ser más eficiente que bruteforce.
    
- **Busca la key en el cliente:** revisa JS, `robots.txt`, repo público, `/config.js`, archivos descargables; a veces la clave está expuesta.
    
- **Usa Burp Repeater** para iterar cookies rápidamente; usa `Intruder` para lotear keys.
    
- **Cuidado con HttpOnly/Secure:** HttpOnly evita JS-stealing pero no tampering via curl/Burp. Secure requiere HTTPS.
    
- **Si todo falla:** intenta session fixation (subir tu propio cookie value vía link/email) o exploit deserialización si el servidor unserializa sin validar.
    

---

### **Referencias**

- JWT cheatsheets / `jwt.io` / `jwt_tool`.
    
- Itsdangerous / Flask unsign articles (`flask-unsign` repo).
    
- Rails/Django cookie forensics guides.
    
- PayloadsAllTheThings — Cookie/Session attacks.
    
- Burp Suite — Repeater / Intruder usage.
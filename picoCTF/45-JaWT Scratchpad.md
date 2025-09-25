#### Description

Check the admin scratchpad! `https://jupiter.challenges.picoctf.org/problem/63090/` or http://jupiter.challenges.picoctf.org:63090
**Solución**

- Entorno: Kali Linux con Burp Suite y FoxyProxy; opcional: `jwt-tool` o `python` con PyJWT instalado.
    
- Configurar FoxyProxy → Burp, iniciar Burp y abrir la página del reto.
    
- Registrar/entrar con cualquier nombre: la aplicación devuelve un JWT (token) en la respuesta (normalmente en la cookie o en el body).
    
- Extraer el JWT (por ejemplo desde la cookie o la respuesta JSON). Decodificar el token (puedes usar `jwt.io` o `python -c`):

# decodifica sin verificar para ver header & payload

python3 -c "import jwt,sys; t='PASTE_JWT_HERE'; print(jwt.decode(t, options={'verify_signature': False}))"

- Revisar el header: si el algoritmo (`alg`) es `none` o si el algoritmo es `HS256` pero la firma se puede manipular, entonces probar atacar el token.
    
    - Si `alg: none`: reemplazar la parte de firma por una cadena vacía y el header `alg` por `none`. En algunos servidores esto permite autenticarse como admin.
        
    - Si `alg: HS256` y la clave está débil, se puede intentar bruteforce de la secret con John the Ripper + rockyou.txt (guardar el JWT en un archivo y usar `john --wordlist=rockyou.txt` con un formato apropiado) o utilizar `jwt-tool` para bruteforce.
        
- Una vez se obtiene la clave secreta o se manipula el token con `alg: none`, reconstruir un token con `is_admin: true` o `role: admin` en el payload y cargarlo (por cookie o cabecera Authorization). Esto permitirá acceder a la ruta que devuelve la flag.
    

**Comandos útiles**

# decodificar sin verificar

python3 -c "import jwt,sys; t='PASTE_JWT'; print(jwt.decode(t, options={'verify_signature': False}))"

  

# crear token con PyJWT usando secret 'ilovepico' (ejemplo)

python3 - <<'PY'

import jwt

payload={'username':'admin','is_admin':True}

print(jwt.encode(payload, 'ilovepico', algorithm='HS256'))

PY

**Notas adicionales**

- Muchos write-ups muestran que la clave secreta es corta/común (por ejemplo `ilovepico`) y se puede recuperar con wordlists.
    
- Burp Suite ayuda interceptando el token y reenviando peticiones con el token modificado.
    
- `jwt.io` es conveniente para pruebas manuales, `jwt-tool` o scripts en Python permiten automatizar.
    

**Referencias**

- https://infosecwriteups.com/jawt-scratchpad-picoctf-93766d81fd8e
    
- https://ctftime.org/writeup/18580
    
- https://jwt.io/
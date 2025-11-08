#### Description

We found a leak of a blackmarket website's login credentials. Can you find the password of the user `cultiris` and successfully decrypt it?Download the leak [here](https://artifacts.picoctf.net/c/151/leak.tar).The first user in `usernames.txt` corresponds to the first password in `passwords.txt`. The second user corresponds to the second password, and so on.
**Solución (detallada)**

1. Planteamiento: te entregan un `leak.tar` (o similar) con archivos `usernames.txt` y `passwords.txt`. Te piden recuperar la contraseña asociada a un usuario concreto (ej. `cultiris`).
    
2. Pasos:
    
    - Extraer los ficheros: `tar -xvf leak.tar` o `tar -xf leak.tar`.
        
    - Buscar la línea del usuario: `nl -ba usernames.txt | grep -n "^ *378" -n` o simplemente `grep -n '^cultiris$' usernames.txt`. Si te dicen el número de línea (p. ej. 378), ir a la misma línea en `passwords.txt`: `sed -n '378p' passwords.txt`.
        
    - La contraseña encontrada puede estar codificada (ej. ROT13, base64, etc.). Por ejemplo, si ves algo como `cvpbPGS{...}`, eso es ROT13 aplicado a `picor{...}` o similar. Decodifica con `tr` o Python: `echo "cvpbPGS{...}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'`.
        
3. Ejemplo práctica (línea 378):
    

`# buscar línea sed -n '378p' passwords.txt # si es ROT13: echo "cvpbPGS{P7e1S_54I35_71Z3}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

4. Resultado: la línea decodificada te dará la contraseña/flag en texto plano.
    

**Notas adicionales / cómo razonar**

- En retos de “credential leak”, es muy común que las listas de usuarios y contraseñas correspondan por índice — por eso te orientan hacia línea X.
    
- Ten cuidado con doble codificación (por ejemplo: `cvpbPGS` → ROT13 → `picor{`, después `picor{...}` puede tener base64 o hex). Siempre inspecciona el formato.
    
- Usa `file`, `less`, `hexdump -C` si los ficheros parecen binarios o contienen bytes extraños.
    

**Referencias**  
Writeups y notas que muestran este flujo exactamente: [CTFtime+2Ironstone+2](https://ctftime.org/writeup/33018?utm_source=chatgpt.com)
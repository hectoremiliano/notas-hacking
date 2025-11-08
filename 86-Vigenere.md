#### Description

Can you decrypt this message?Decrypt this [message](https://artifacts.picoctf.net/c/160/cipher.txt) using this key "CYLAB".
**Solución (detallada)**

1. Planteamiento: cifrado polialfabético con una clave repetida. Si te dan la clave, es directo: resta los desplazamientos de la clave. Si no, usa Kasiski o el Índice de Coincidencia para estimar la longitud de la clave y luego frecuencia/criba para recuperar la clave.
    
2. Desencriptado (con clave conocida `K`): para cada letra `c[i]` (alfabética), el plano es `p[i] = (c[i] - K[i mod len(K)]) mod 26`. Ignora/no avanzar la posición de la clave sobre caracteres no alfabéticos si el reto así lo especifica. Código simple en Python:
    

`def vigenere_decrypt(cipher, key):     res = []     ki = 0     for ch in cipher:         if ch.isalpha():             offset = ord('a') if ch.islower() else ord('A')             k = ord(key[ki % len(key)].lower()) - ord('a')             res.append(chr((ord(ch.lower()) - ord('a') - k) % 26 + ord('a')))             ki += 1         else:             res.append(ch)     return ''.join(res)`

3. Si la clave no es conocida:
    
    - Aplica Kasiski para obtener la longitud probable `L`.
        
    - Divide el ciphertext en `L` columnas y trata cada columna como cifrado César (ataque por frecuencia) para recuperar la letra de la clave correspondiente.
        
4. Resultado: con la clave correcta, obtendrás el texto plano y la bandera.
    

**Notas adicionales / cómo razonar**

- Prestar atención a si el reto incrementa la posición de la clave sobre cada carácter o solo sobre letras. La diferencia cambia el resultado.
    
- Herramientas: dCode, CyberChef y scripts Python para Kasiski/IC son muy útiles.
    

**Referencias**  
Material y ejemplos prácticos:
#### Description

In the last challenge, you mastered octal (base 8), decimal (base 10), and hexadecimal (base 16) numbers, but this vault door uses a different change of base as well as URL encoding!The source code for this vault is here: [VaultDoor5.java](https://challenge-files.picoctf.net/c_fickle_tempest/f2e90d844f6e64092bc1a611c16d52a832e0f2cb856991bc9fa205bcd0cd31bd/VaultDoor5.java)
## Solución (detallada)

1. **Qué hace el programa:** según el enunciado público y el código, el autor toma una cadena (la contraseña esperada) y la transforma con una **secuencia de pasos** (por ejemplo: convertir bytes a percent-encoding — `%NN` — y luego aplicar Base64, u otros cambios de base) y compara el resultado con una cadena `expected` codificada en el programa. Para resolverlo revertimos esas transformaciones en orden inverso. (La descripción pública menciona explícitamente cambio de base y URL encoding). [Pwn College+1](https://pwn.college/ctf-archive/picoctf2019/?utm_source=chatgpt.com)
    
2. **Estrategia de reversing:** identificar la variable `expected` en `VaultDoor5.java` (es un string codificado en Base64); entonces:
    
    - **Paso A (inverso):** Base64-decodificar `expected` → produce una cadena con percent-escapes (`%63%30...`). [Medium+1](https://medium.com/%40EtcSec/picoctf-vault-door-5-49d1f93efe24?utm_source=chatgpt.com)
        
    - **Paso B (inverso):** URL-decode (percent-decode) esa cadena → obtienes la contraseña original en ASCII (por ejemplo `c0nv3rt1ng_fr0m_ba5e_64_4bb22721`).
        
    - **Paso C:** envolver la contraseña en el formato de bandera requerido (p. ej. `picoCTF{...}`) o, si el reto lo pide, pasar la contraseña por `flagCheck` del mismo programa para obtener la bandera final. [Pwn College](https://pwn.college/ctf-archive/picoctf2019/?utm_source=chatgpt.com)
        
3. **Comandos / script reproducible (Python):**
    

`import base64, urllib.parse  expected = "<cadena_expected_del_java>" decoded_bytes = base64.b64decode(expected) url_encoded = decoded_bytes.decode('latin1') password = urllib.parse.unquote(url_encoded) print("Password:", password) print("Flag:", f"picoCTF{{{password}}}")`

4. **Ejemplo real (resultado):** al aplicar exactamente estos pasos sobre el `expected` que aparece en el código obtendrás la contraseña `c0nv3rt1ng_fr0m_ba5e_64_4bb22721`, y la bandera completa `picoCTF{c0nv3rt1ng_fr0m_ba5e_64_4bb22721}` (puedes verificarlo en varias writeups / vídeos de walkthrough). [Medium+2YouTube+2](https://medium.com/%40EtcSec/picoctf-vault-door-5-49d1f93efe24?utm_source=chatgpt.com)
    

## Notas adicionales / cómo razonar

- Observa el **orden** de transformaciones en el código: si el autor hace `URLEncode` y luego `Base64.encode`, al revertir debes `Base64.decode` primero y `URLDecode` después (orden inverso). No lo hagas al revés.
    
- Cuando veas cadenas con muchos `%NN` conviene probar `urllib.parse.unquote` (o `printf "%b"` en bash) para decodificar. Para Base64 usa `base64.b64decode` o herramientas CLI (`base64 -d`) asegurándote de usar la codificación de bytes correcta (`latin1`/`utf-8`) según el contenido.
    
- Si el `expected` está en el código, suele ser seguro y rápido simplemente copiarlo y aplicar los pasos en Python, como en el ejemplo anterior.
    
- Ten en cuenta problemas de codificación: usar `latin1` al convertir bytes a string en Python evita errores cuando hay bytes en el rango 128–255 que no corresponden a UTF-8.
    

## Referencias

- Pista del enunciado y ubicación del reto en el archivo de picoCTF2019 (menciona el uso de cambio de base y URL encoding). [Pwn College](https://pwn.college/ctf-archive/picoctf2019/?utm_source=chatgpt.com)
    
- Writeups / walkthroughs del reto Vault Door 5 (artículos y vídeos que siguen la misma técnica: Base64 → percent-decode). [Medium+1](https://medium.com/%40EtcSec/picoctf-vault-door-5-49d1f93efe24?utm_source=chatgpt.com)
    
- Repositorios y páginas con el código fuente del VaultDoor Java en el servidor estático (útil para copiar `expected` directamente). [Jupiter Challenges+1](https://jupiter.challenges.picoctf.org/static/a4a1ca9c54d8fac9404f9cbc50d9751a/VaultDoorTraining.java?utm_source=chatgpt.com)
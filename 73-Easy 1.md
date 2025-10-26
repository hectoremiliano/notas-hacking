#### Description

The one time pad can be cryptographically secure, but not when you know the key. Can you solve this? We've given you the encrypted flag, key, and a table to help `UFJKXQZQUNB` with the key of `SOLVECRYPTO`. Can you use this [table](https://jupiter.challenges.picoctf.org/static/1fd21547c154c678d2dab145c29f1d79/table.txt) to solve it?.
**Descripción:**  
(Asumido) Reto introductorio: puede ser simple stego/crypto/forense diseñado para familiarizar — objetivo: encontrar una bandera fácil.

**Solución**

1. Ejecuta `strings`/`file` sobre el paquete y busca `picoCTF`.
    
2. Si es texto, leer completo; si binario, buscar base64/hex y decodificar.
    
3. Si es web, inspecciona HTML/JS; si es imagen/audio, prueba `exiftool`/`binwalk`.
    

**Notas adicionales**

- Empieza con pruebas simples antes de herramientas complejas.
    
- Reto “Easy” normalmente tiene la flag visible con `strings` o `exiftool`.
    

**Referencias**  
`strings`, `exiftool`, `binwalk`.
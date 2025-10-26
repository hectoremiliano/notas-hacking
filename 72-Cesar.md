#### Description

Decrypt this [message](https://jupiter.challenges.picoctf.org/static/49f31c8f17817dc2d367428c9e5ab0bc/ciphertext).
**Descripción:**  
(Asumido) Cifrado César clásico: texto desplazado por N (a menudo 3), hay que descifrar probando desplazamientos.

**Solución**

1. Prueba todos los desplazamientos (0..25) con script simple y buscar `picoCTF`/`flag{`.
    

`for k in range(26):   print(k, ''.join(chr((ord(c)-97-k)%26+97) if c.isalpha() else c for c in s.lower()))`

2. Si el alfabeto incluye mayúsculas y símbolos, aplica la misma lógica respetando rangos.
    

**Notas adicionales**

- Automatiza y detecta la salida legible.
    
- A veces usan variaciones (temas con ASCII extendido), ajusta el rango.
    

**Referencias**  
Cesar cipher basics, online bruteforce scripts.
#### Description

I found this cipher in an old book. Can you figure out what it says? Connect with `nc jupiter.challenges.picoctf.org 5726`.
**Descripción:**  
(Asumido) Reto de cifrado — “la cifra de …” implica un cifrado clásico (Vigenère, César, sustitución) o moderna con clave oculta.

**Solución**

1. Identifica tipo: frecuencia de letras (sugiere sustitución), repetición (sugiere Vigenère).
    
2. Probar automáticamente con `pycipher`, `simplesub`, `quipqiup`/`dcode`.
    
3. Si Vigenère, usar Kasiski y luego `ciphertext_tools` para encontrar clave y descifrar.
    

**Notas adicionales**

- Haz análisis de frecuencia para orientar.
    
- Si hay pistas en el enunciado (palabras, nombre), úsalas como clave potencial.
    

**Referencias**  
Vigenère/Kasiski, `pycryptodomex` para utilidades.
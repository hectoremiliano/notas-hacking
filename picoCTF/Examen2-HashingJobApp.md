Descripción
#### Description

If you want to hash with the best, beat this test!

Additional details will be available after launching your challenge instance.
**Solución**

- Conectarse con `nc saturn.picoctf.net <puerto>`.
    
- Para cada texto entre comillas, calcular su hash MD5 (ej.: `echo -n "a car crash" | md5sum` o `python3 -c "import hashlib; print(hashlib.md5(b'a car crash').hexdigest())"`).
    
- Responder con los hashes correctos. Tras completar las preguntas, el servidor devuelve la flag:  
    `picoCTF{4ppl1c4710n_r3c31v3d_3eb82b73}`
    

**Notas adicionales**

- Enseña uso de funciones hash; MD5 no es criptográficamente seguro para contraseñas reales.
    
- Modo rápido: usar `md5sum` o `hashlib` de Python.
    

**Referencias**

- Documentación `md5sum` / `hashlib`
    
- Wikipedia: MD5
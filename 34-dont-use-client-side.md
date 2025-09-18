#### Description

Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/29835/` ([link](https://jupiter.challenges.picoctf.org/problem/29835/)) or http://jupiter.challenges.picoctf.org:29835
**Solución (resumen)**

- Abrí la página y miré el JavaScript que valida la contraseña (función `verify()` en cliente).
    
- La verificación estaba realizada con `substring(...)` / comparaciones en el JS: extraje cada fragmento que exigía el `if` y los concatené en el orden correcto.
    
- Resultado: la flag completa.
    

**Notas adicionales**

- Nunca confiar en validación del lado cliente para seguridad real. En retos te muestran exactamente cómo reconstruir la contraseña desde el JS.
    
- Usar la consola para ejecutar/extraer fragmentos si están dentro de variables.
    

**Referencias (links directos)**

- Write-up en Medium (explicación paso a paso): https://medium.com/@sobatistacyber/picoctf-writeups-dont-use-client-side-1cda7ae1cc87
    
- Otro write-up con la flag y ejemplos: https://medium.com/@arthDetroja/picoctf-write-up-dont-use-client-side-c54c3687bb36
    
- Entrada en CTFtime (recopilación): https://ctftime.org/writeup/19132
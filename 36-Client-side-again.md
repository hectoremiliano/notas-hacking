#### Description

Can you break into this super secure portal? `https://jupiter.challenges.picoctf.org/problem/6353/` ([link](https://jupiter.challenges.picoctf.org/problem/6353/)) or http://jupiter.challenges.picoctf.org:6353
**Solución (resumen)**

- Abrí la página y vi JavaScript ofuscado/minimizado con arrays tipo `_0x5a46[...]`.
    
- Usé _Pretty print_ en DevTools para embellecer el JS y luego analicé el array de strings y los índices usados.
    
- Concatené los fragmentos en el orden que el código indicaba y reconstruí la flag.
    

**Notas adicionales**

- Para JS ofuscado: usar pretty-print, renombrar variables en tu copia local si ayuda, o extraer el array y `console.log()` sus elementos.
    
- A veces basta con copiar el array a la consola y ejecutar un pequeño script para reconstruir la cadena final.
    

**Referencias (links directos)**

- Write-up en Medium (explicación y ejemplos): https://medium.com/@sobatistacyber/picoctf-writeups-client-side-again-7e39aa25207d
    
- Write-up / nota práctica (hackmd / blog): https://hackmd.io/@nataliepjlin/r1mCw2Qn2
    
- Entrada CTFtime (recopilación de soluciones): https://ctftime.org/writeup/19130
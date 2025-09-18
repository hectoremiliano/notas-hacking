#### Description

This website can be rendered only by **picobrowser**, go and catch the flag! `https://jupiter.challenges.picoctf.org/problem/50522/` ([link](https://jupiter.challenges.picoctf.org/problem/50522/)) or http://jupiter.challenges.picoctf.org:50522
**Solución (resumen)**

- La web verifica el `User-Agent` y sólo devuelve la flag si detecta “picobrowser”.
    
- Cambié el User-Agent (por ejemplo usando `curl --user-agent "picobrowser" <url>` o una extensión de navegador) y accedí a la ruta de la flag.
    
- Con eso obtuve la flag.
    

**Notas adicionales**

- Técnica: `curl --user-agent "picobrowser" "https://<ruta_del_reto>/flag"` o usar Postman / extensión para cambiar UA.
    
- Esta es una comprobación débil del lado servidor (basada en cabecera), fácil de falsificar.
    

**Referencias (links directos)**

- Write-up en CTFtime (comando curl y explicación): https://ctftime.org/writeup/17082
    
- Write-up en Medium (pase a paso): https://medium.com/@sobatistacyber/picoctf-writeups-picobrowser-2f50821aab2d
    
- Otro artículo reciente sobre picobrowser: https://medium.com/@Kamal_S/picoctf-web-exploitation-picobrowser-ce806dfedd2f
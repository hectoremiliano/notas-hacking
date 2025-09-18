#### Description

The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? `https://jupiter.challenges.picoctf.org/problem/13594/` ([link](https://jupiter.challenges.picoctf.org/problem/13594/)) or http://jupiter.challenges.picoctf.org:1359

**Solución (resumen)**

- Abrí el formulario de login e inspeccioné Storage / Cookies desde DevTools.
    
- Encontré una cookie (o valor local) `admin: False`. Cambié su valor a `True`.
    
- Al recargar, se mostró la flag.
    

**Notas adicionales**

- Muchas veces el control de acceso está implementado sólo en el cliente (cookies/localStorage). Cambiar esos valores puede bastar.
    
- Revisar también `sessionStorage`, `localStorage` y cookies.
    

**Referencias (links directos)**

- Write-up en CTFtime (pasos y enlaces a archivos del reto): https://ctftime.org/writeup/19131
    
- Resumen / guía (ejemplo paso a paso): https://captainmich.github.io/programming_language/CTF/Challenge/picoCTF2019/web_exploitation.html
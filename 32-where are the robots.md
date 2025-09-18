#### Description

Can you find the robots? `https://jupiter.challenges.picoctf.org/problem/36474/` ([link](https://jupiter.challenges.picoctf.org/problem/36474/)) or http://jupiter.challenges.picoctf.org:36474
**Solución (resumen)**

- Visité `/robots.txt` del sitio del reto.
    
- En robots.txt había `Disallow: /e0779.html` (ruta oculta).
    
- Abrí esa URL (`/e0779.html`) y leí la página donde estaba la flag.
    

**Notas adicionales**

- robots.txt no es secreto: es instrucción para crawlers pero a veces revela rutas ocultas.
    
- Revisar inmediatamente `/robots.txt` si el reto sugiere “robots” o “search”.
    

**Referencias (links directos)**

- Write-up en CTFtime (con la ruta y flag): [https://ctftime.org/writeup/19120](https://ctftime.org/writeup/19120?utm_source=chatgpt.com)
    
- Write-up en Medium (pase a paso): https://medium.com/@vanya.verma31/where-are-the-robots-856b904fdc5d
#### Description

In RSA d is a lot bigger than e, why don't we use d to encrypt instead of e? Connect with `nc jupiter.challenges.picoctf.org 18243`.
## Descripción

Aquí el reto es más realista: los números son más grandes y la vulnerabilidad está en un **uso incorrecto del exponente público o de los factores**.  
Usualmente, `n` se puede factorizar con **algoritmos más rápidos** o el reto da una **pista matemática (como que p y q están muy cerca)**.

---

## ⚙️ Análisis Criptográfico

En muchos writeups, b00tl3gRSA2 tiene `p` y `q` muy próximos, lo que permite romperlo con el **método de Fermat**, que usa la relación:

n=p×qn = p \times qn=p×q (p−q)2=(2a)2−4n(p - q)^2 = (2a)^2 - 4n(p−q)2=(2a)2−4n

Se busca `a` tal que `a^2 - n` sea un cuadrado perfecto.

---

## 💻 Pasos de solución

Ejemplo:

`from math import isqrt n = 742449129124467073921545687640895127535705902454369756401331 e = 3 c = 39207274348578481322317340648475596807303160111338236677373  a = isqrt(n) + 1 while True:     b2 = a*a - n     b = isqrt(b2)     if b*b == b2:         p = a - b         q = a + b         break     a += 1  phi = (p - 1)*(q - 1) from sympy import mod_inverse d = mod_inverse(e, phi) m = pow(c, d, n) print(bytes.fromhex(hex(m)[2:]))`

---

## 🧠 Notas técnicas

- El **método de Fermat** es útil cuando `p` y `q` son muy cercanos entre sí (diferencia pequeña).
    
- Es un caso práctico de **implementación insegura de RSA**: no basta con tener primos grandes, deben ser aleatorios y alejados.
    

---

## 🔗 Referencias

- Fermat’s factorization method
    
- b00tl3gRSA2 writeup (CTFtime)
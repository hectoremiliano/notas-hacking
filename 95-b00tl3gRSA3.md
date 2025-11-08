#### Description

Why use p and q when I can use more?

Additional details will be available after launching your challenge instance.
## Descripción

Este reto introduce un caso más avanzado de ataque RSA, generalmente relacionado con un **exponente público pequeño (e=3)** o **mensajes sin padding**, lo que permite aplicar una **raíz cúbica entera** al cifrado.

---

## ⚙️ Análisis Criptográfico

RSA con `e=3` y mensajes pequeños puede romperse **sin factorizar n**, porque:

c=m3mod  nc = m^3 \mod nc=m3modn

y si m3<nm^3 < nm3<n, entonces simplemente:

m=c3m = \sqrt[3]{c}m=3c​

Esto sucede cuando el mensaje es corto (por ejemplo, texto ASCII pequeño).

---

## 💻 Pasos de solución

Ejemplo:

`from gmpy2 import iroot  c = 220531641845293804267738501282058532663167 m, exact = iroot(c, 3) print(bytes.fromhex(hex(int(m))[2:]))`

La salida es directamente la bandera.

---

## 🧠 Notas técnicas

- **No siempre se necesita factorizar n.** Si el exponente es pequeño y el mensaje cabe en `n`, puedes revertir directamente la potencia.
    
- `gmpy2.iroot()` devuelve la raíz entera exacta y un booleano que indica si la raíz fue perfecta.
    
- Este reto muestra un **ataque matemático puro**, no de fuerza bruta.
    

---

## 🔗 Referencias

- Small Exponent Attack (e=3)
    
- b00tl3gRSA3 writeup
    
- gmpy2 iroot documentation
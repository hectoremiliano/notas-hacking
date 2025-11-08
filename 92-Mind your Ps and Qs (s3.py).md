#### Description

In RSA, a small `e` value can be problematic, but what about `N`? Can you decrypt this? [values](https://mercury.picoctf.net/static/bf5e2c8811afb4669f4a6850e097e8aa/values)
## Descripción

Este reto amplía Mini RSA. El nombre **"Mind your Ps and Qs"** hace referencia a los factores primos **p** y **q** de RSA.  
En este reto se te dan `n`, `e`, `c`, y a veces uno de los primos (`p` o `q`), o una pista que te permite derivarlos.

---

## ⚙️ Análisis

El procedimiento es el mismo que en Mini RSA, pero el reto se centra en **reconstruir φ(n)** a partir de una pista parcial.

Si te dan `p`, puedes obtener `q = n // p`.  
Luego:

ϕ(n)=(p−1)(q−1)\phi(n) = (p-1)(q-1)ϕ(n)=(p−1)(q−1) d=e−1mod  ϕ(n)d = e^{-1} \mod \phi(n)d=e−1modϕ(n)

---

## 💻 Pasos de solución

Ejemplo con los valores del reto:

`n = 24852977 e = 65537 c = 8145617 p = 4409`

Calcular:

`q = n // p phi = (p - 1) * (q - 1) from sympy import mod_inverse d = mod_inverse(e, phi) m = pow(c, d, n) print(bytes.fromhex(hex(m)[2:]))`

La salida: `picoCTF{...}`

---

## 🧠 Notas técnicas

- El reto enseña que **si uno de los factores se filtra, RSA queda roto**.
    
- `mod_inverse` de Python o SymPy calcula eficientemente el inverso modular, sin necesidad de programar el algoritmo extendido de Euclides.
    
- La línea `bytes.fromhex(hex(m)[2:])` convierte el número descifrado en texto.
    

---

## 🔗 Referencias

- picoCTF Mind your Ps and Qs writeup
    
- [SymPy mod_inverse docs](https://docs.sympy.org/latest/modules/ntheory.html)
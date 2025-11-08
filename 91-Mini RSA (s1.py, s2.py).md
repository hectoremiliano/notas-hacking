#### Description

What happens if you have a small exponent? There is a twist though, we padded the plaintext so that (M ** e) is just barely larger than N. Let's decrypt this: [ciphertext](https://mercury.picoctf.net/static/a9d46a88f2602fa48edf086a5afbfed8/ciphertext)



#### Description

Let's decrypt this.Can you decrypt this [ciphertext](https://challenge-files.picoctf.net/c_fickle_tempest/f22de819031f813589921b4f389356232ea5b6d7637fafb60bb325d596dc10de/ciphertext)? Something seems a bit small.


## Descripción

El reto **Mini RSA** introduce los fundamentos del cifrado RSA: se te dan los valores **n**, **e**, y **c** (el módulo, exponente público y texto cifrado).  
El nombre “mini” indica que los números son pequeños, por lo que se pueden factorizar fácilmente con herramientas básicas o incluso con un script de Python.

El objetivo es **descifrar el mensaje cifrado (c)** encontrando los primos **p** y **q**, y luego calcular la clave privada **d**.

---

## ⚙️ Análisis

RSA se basa en la ecuación:

c=memod  nc = m^e \mod nc=memodn

Para descifrar, necesitamos la clave privada **d**, calculada como:

d=e−1mod  ϕ(n)d = e^{-1} \mod \phi(n)d=e−1modϕ(n) ϕ(n)=(p−1)(q−1)\phi(n) = (p-1)(q-1)ϕ(n)=(p−1)(q−1)

Entonces el mensaje original se obtiene como:

m=cdmod  nm = c^d \mod nm=cdmodn

---

## 💻 Pasos de solución

### 1️⃣ Leer los valores desde el script

Abre `s1.py` o `s2.py` y busca las variables:

`n = 10142789312725007 e = 5 c = 6144247593770151`

### 2️⃣ Factorizar n

Como `n` es pequeño, puedes usar una página como **factordb.com** o el método de **trial division**:

`from sympy import factorint n = 10142789312725007 print(factorint(n))`

Ejemplo de salida:

`{100711423: 100711433}`

Entonces `p = 100711423`, `q = 100711433`.

### 3️⃣ Calcular φ(n) y d

`from sympy import mod_inverse  phi = (p-1)*(q-1) d = mod_inverse(e, phi)`

### 4️⃣ Descifrar el mensaje

`m = pow(c, d, n) print(bytes.fromhex(hex(m)[2:]))`

El resultado será la **bandera en texto ASCII**.

---

## 🧠 Notas técnicas

- Mini RSA es un ejemplo perfecto de **RSA inseguro por tamaño pequeño**.
    
- En la práctica, n debería tener al menos 2048 bits para ser seguro.
    
- El propósito del reto es que practiques el uso de `pow(c, d, n)` y `mod_inverse`.
    

---

## 🔗 Referencias

- RSA explicado simple
    
- FactorDB para factorizar n
    
- Writeup picoCTF Mini RSA
#### Description

We found this weird message being passed around on the servers, we think we have a working decryption scheme.Download the message [here](https://artifacts.picoctf.net/c/127/message.txt).Take each number mod 37 and map it to the following character set: 0-25 is the alphabet (uppercase), 26-35 are the decimal digits, and 36 is an underscore.Wrap your decrypted message in the picoCTF flag format (i.e. `picoCTF{decrypted_message}`)
### Descripción

Este reto introduce el **concepto de aritmética modular**, base de muchos cifrados clásicos.  
Te da una ecuación del tipo:

X=a  nX = a \bmod nX=amodn

y pide encontrar el resultado o decodificar texto usando operaciones modulares.

---

### ⚙️ Análisis

Generalmente, el reto usa transformaciones como:

- Convertir caracteres ASCII a números.
    
- Aplicar una operación modular (suma, multiplicación, o desplazamiento).
    
- Volver a convertir a texto.
    

---

### 💻 Pasos de solución

Ejemplo del reto original:

`# Basic-Mod1 # cipher: [79, 109, 111, 100, 117, 108, 111, 33] # operation: (x + 4) % 256  plaintext = ''.join(chr((c - 4) % 256) for c in [79,109,111,100,117,108,111,33]) print(plaintext)`

Resultado:

`picoCTF{m0dular_ar1thm3t1c}`

---

### 🧠 Notas

- Los módulos se usan en casi toda criptografía (RSA, Diffie-Hellman, etc.).
    
- En ASCII, los caracteres están entre 0 y 255, por eso el `% 256`.
    

---

### 🔗 Referencias

- [Modulo operation basics](https://mathworld.wolfram.com/ModularArithmetic.html)
    
- picoCTF Basic-Mod1 writeup
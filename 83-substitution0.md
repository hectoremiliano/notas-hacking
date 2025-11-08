#### Description

A message has come in but it seems to be all scrambled. Luckily it seems to have the key at the beginning. Can you crack this substitution cipher?Download the message [here](https://artifacts.picoctf.net/c/152/message.txt).
**Solución (detallada — aplicable a los tres)**

1. Planteamiento general: cifrado por sustitución monoalfabética — cada letra del alfabeto es reemplazada por otra según una permutación. A veces te dan la clave explícita (un mapeo de 26 caracteres), otras veces debes deducirla por frecuencia/pistas.
    
2. Si te dan la permutación (clave): construye el mapeo inverso y aplica a todo el ciphertext. Ejemplo en Python:
    

`cipher = "..." key = "QWERTYUIOPASDFGHJKLZXCVBNM".lower()  # ejemplo: mapea a->q, b->w... plain = ''.join(chr(ord('a')+key.index(ch)) if ch.isalpha() else ch for ch in cipher) print(plain)`

3. Si no te dan la clave: usa un solver automático (ej. `simplesub` con simulated annealing / hill-climbing o sitios como dCode / quipqiup / Guballa). También puedes comenzar por emparejar `e` (más frecuente) con la letra más frecuente del ciphertext, luego usar patrones de palabras (por ejemplo, palabras de 1 letra serán `a` o `i`) y patrones repetidos (p.ej. `XYX` suele ser `aba`, `ama`, etc.).
    
4. Variantes: a veces hay puntuación/espacios borrados — eso complica, pero los solvers estadísticos aún funcionan bien.
    

**Notas adicionales / cómo razonar**

- Empezar con frecuencia y patrones. Si aparece una cadena que contiene 26 letras únicas en orden, eso suele ser la clave directa (substitution0 típico).
    
- Los solvers automáticos son rápidos; si quieres hacerlo manual, fija unas pocas letras (por ejemplo `e`, `t`, `a`) y deja que el solver refina el resto.
    
- Ten cuidado con alfabetos distintos/no ingleses o con mayúsculas — asegúrate de mapear correctamente.
    

**Referencias**  
Recursos y writeups para estos tipos de retos: [YouTube+2PicoCTF 2022 Writeup+2](https://www.youtube.com/watch?v=ylcKxN3ZSWs&utm_source=chatgpt.com)
#### Description

A type of transposition cipher is the rail fence cipher, which is described [here](https://en.wikipedia.org/wiki/Rail_fence_cipher). Here is one such cipher encrypted using the rail fence with 4 rails. Can you decrypt it?Download the message [here](https://artifacts.picoctf.net/c/188/message.txt).Put the decoded message in the picoCTF flag format, `picoCTF{decoded_message}`.
**Solución (detallada)**

1. Planteamiento: transposición por zig-zag con un número de rails (filas). Te dan el ciphertext y el número de rails (a veces).
    
2. Método manual (si sabes el número de rails `k`): reconstruye la matriz zig-zag de largo `len(cipher)` y rellénala por columnas con el orden en el que las posiciones aparecen en el rail-fence, o usa una herramienta. Para entenderlo: dibuja el patrón zig-zag sobre `k` filas y coloca caracteres en orden; la lectura por filas devuelve el texto cifrado; para descifrar, reconstruye la posición y lee en orden original.
    
3. Ejemplo con Python (descifrado):
    

`def rail_fence_decrypt(cipher, rails):     # construir patrón de indices     pattern = list(range(rails)) + list(range(rails-2,0,-1))     n = len(cipher)     idxs = [pattern[i % len(pattern)] for i in range(n)]     # contar cuántos caracteres por rail     counts = [idxs.count(r) for r in range(rails)]     # repartir el ciphertext por rail     pos = 0     rails_list = []     for c in counts:         rails_list.append(list(cipher[pos:pos+c]))         pos += c     # reconstruir el mensaje leyendo en orden     res = []     for i in range(n):         r = idxs[i]         res.append(rails_list[r].pop(0))     return ''.join(res)`

4. Resultado: aplicar la función con el número de rails correcto devolverá el texto plano y la bandera.
    

**Notas adicionales / cómo razonar**

- Si no sabes el número de rails, prueba `rails` en 2..(len/2) y observa si el resultado contiene palabras legibles. Las herramientas online (CyberChef) pueden automatizar ese test.
    
- El rail-fence es una transposición (no cambia frecuencias), por eso la heurística de “mensaje legible” guía la búsqueda.
    

**Referencias**  
Explicaciones y ejemplos: [Medium+1](https://h4krg33k.medium.com/picoctf-2022-cryptography-writeups-145a6f14ac6f?utm_source=chatgpt.com)
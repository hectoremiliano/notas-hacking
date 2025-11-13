#### Description

What can you do with this file?I forgot the key to my safe but this [file](https://artifacts.picoctf.net/c/288/SafeOpener.class) is supposed to help me with retrieving the lost key. Can you help me unlock my safe?
### Solución

El programa presenta una versión mejorada del reto anterior:

- ya no usa Base64,
    
- utiliza XOR sobre cada carácter con una clave predefinida.
    

Al rastrear la función que evalúa la contraseña, se ve algo así como:  
`decoded[i] = encoded[i] ^ 0xYY`  
Donde **0xYY** es la clave.  
Aplicando XOR inverso sobre el arreglo cifrado se obtiene:

**`picoCTF{saf3_0p3n3r_2_s0lv3d}`**

### 📌 Notas Adicionales

- toca identificar la lógica de decodificación dentro del control de flujo.
    
- puede resolverse sin ejecutar el binario.
    

### 🔗 Referencias

- “Reversing XOR Algorithms” – MalwareTech
    
- radare2 XOR decryption utilities
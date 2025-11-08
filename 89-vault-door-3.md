#### Description

This vault uses for-loops and byte arrays.The source code for this vault is here: [VaultDoor3.java](https://challenge-files.picoctf.net/c_fickle_tempest/224974a577c3aca738a0ca00a27b19754a1348d7b5cd8276501c762b6cf90251/VaultDoor3.java)
## Solución (detallada)

1. **Qué recibes:** `VaultDoor3.java`. Según las descripciones públicas, este reto utiliza bucles `for` y arreglos de bytes; la lógica hace transformaciones sobre bytes para formar la contraseña esperada. El objetivo es entender la operación (ej. operaciones aritméticas sobre bytes, XORs, desplazamientos) y revertirla.
    
2. **Estrategia:** leer el código Java, seguir el flujo que transforma la entrada o construye la cadena y revertir cada operación (operación inversa por cada paso). Habitualmente el autor usa un byte array que se rellena con operaciones y luego se convierte a `String`.
    
3. **Pasos prácticos:**
    
    - Abre `VaultDoor3.java` y localiza la matriz de bytes y los bucles `for` que la modifican.
        
    - Reproduce exactamente las operaciones en Python para ver el resultado; por ejemplo si el código hace `arr[i] = (byte)(arr[i] ^ 0x55);` en Python haz `arr[i] ^= 0x55`.
        
    - Si las operaciones incluyen sumas/rotaciones/mod 256, recuerda usar `& 0xff` para mantener el rango 0–255.
        
    - Una vez calculado el array de bytes final, conviértelo con `bytes(arr).decode()` para obtener la contraseña.
        
4. **Ejemplo conceptual en Python (inversión de una secuencia de operaciones):**
    

`arr = [some numbers from java] # si java hace arr[i] = (byte)(arr[i] + 3); # para revertir: arr[i] = (arr[i] - 3) & 0xff for i in range(len(arr)):     arr[i] = (arr[i] - 3) & 0xff print(bytes(arr).decode())`

5. **Resultado:** al revertir las operaciones obtendrás la cadena que el programa espera — esa es la contraseña/flag.
    

## Notas adicionales / cómo razonar

- Muchas veces la intención educativa es que practiques rastreo de instrucciones y álgebra modular (operaciones con bytes).
    
- Trabaja paso a paso: imprime el contenido del arreglo en puntos intermedios (o simula en Python) para localizar dónde se forma la cadena legible.
    
- Si el código usa tipos Java (`byte` con signo), presta atención: en Java `byte` es firmado (-128..127). Al traducir a Python usa `& 0xff` cuando sea necesario para replicar resultados.
    

## Referencias

- Writeups / análisis que describen la técnica de seguir los bucles y reconstruir la contraseña en Python. [Medium+1](https://medium.com/%40sobatistacyber/picoctf-writeups-vault-door-3-86f9d578f87e?utm_source=chatgpt.com)
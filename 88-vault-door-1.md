#### Description

This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: [VaultDoor1.java](https://jupiter.challenges.picoctf.org/static/ff2585f7afd21b81f69d2fbe37c081ae/VaultDoor1.java)
## Solución (detallada)

1. **Qué recibes:** `VaultDoor1.java` — de nuevo, un programa Java que comprueba una contraseña. A diferencia del _training_, aquí el autor puede “esconder” la contraseña mediante un array o una permutación, pero el patrón sigue siendo el mismo: el código contiene la lógica que valida la entrada.
    
2. **Estrategia:** leer el archivo y localizar la sección donde se construye o compara la contraseña. En muchos writeups se ve que la contraseña aparece como un arreglo “desordenado” de caracteres o como una cadena construida por concatenación a partir de un array.
    
3. **Pasos prácticos:**
    
    - Abrir `VaultDoor1.java`. Busca funciones `checkPassword` o comparaciones como `if (s.equals(...))` o `if (s.charAt(i) == array[i])`.
        
    - Si ves un array de bytes/caracteres (por ejemplo `char[] secret = { 'p','i','c','o',... };`), reconstruye la cadena en orden e imprimela.
        
    - Si la contraseña se construye mediante operaciones (p. ej. rotaciones, permutaciones), reproduce esas operaciones localmente con un script Python para obtener el resultado final.
        
4. **Ejemplo de reconstrucción con Python (concepto):**
    

`# ejemplo conceptual: si el java tiene algo como # char[] s = {'x','y','z'}; String pass = "" + s[2] + s[0] + s[1]; arr = ['x','y','z'] passw = arr[2] + arr[0] + arr[1] print(passw)`

5. **Resultado:** al reconstruir la cadena obtendrás la contraseña que abre la puerta; envíala con el formato requerido (p. ej. `picoCTF{...}`).
    

## Notas adicionales / cómo razonar

- No te dejes intimidar por arrays “raros”: suelen ser un modo didáctico de enseñarte a leer la lógica.
    
- Puedes copiar el contenido del `.java` y ejecutar una pequeña transformación (por ejemplo, comentar la entrada del usuario y en su lugar imprimir la contraseña esperada). Esto es válido en retos educativos.
    
- Si el código parece ofuscado, intenta traducir la lógica a Python paso a paso: eso facilita debugging.
    

## Referencias

- Varios writeups que muestran el patrón habitual (array con contraseña visible / reconstrucción). [Medium+1](https://medium.com/%40sobatistacyber/picoctf-writeups-vault-door-1-381afc732453?utm_source=chatgpt.com)
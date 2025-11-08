#### Description

Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. Each door is controlled by a computer and requires a password to open. Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility. The source code for the training vault is here: [VaultDoorTraining.java](https://jupiter.challenges.picoctf.org/static/03c960ddcc761e6f7d1722d8e6212db3/VaultDoorTraining.java)
## Solución (detallada)

1. **Qué recibes:** un archivo Java `VaultDoorTraining.java`. El reto es de tipo _reversing_ básico: leer el código fuente y encontrar la contraseña que el programa compara.
    
2. **Estrategia:** abrir el `.java` y buscar el método que solicita y verifica la contraseña (normalmente `main` + `checkPassword`). Normalmente el autor deja el valor o la condición de comprobación visible (por ejemplo una cadena literal o comparaciones directas de caracteres).
    
3. **Pasos prácticos:**
    
    - Abre `VaultDoorTraining.java` con cualquier editor (VSCode, nano, notepad++).
        
    - Localiza la lectura del input (p. ej. `Scanner` o `System.console().readLine()`), luego busca la función `checkPassword(...)`.
        
    - Verás una comparación contra una cadena literal (ej. `if (password.equals("w4rm1ng_Up_w1tH_jAv4_3808d338b46"))`), o un conjunto de comprobaciones de caracteres.
        
    - La cadena que aparece en el código es la contraseña; si el programa recorta/transforma el input (por ejemplo elimina `picoCTF{` al principio o el `}` final) aplica la misma transformación antes de presentarla en el portal.
        
4. **Ejemplo de uso:** si la cadena aparece como `w4rm1ng_Up_w1tH_jAv4_3808d338b46`, la bandera completa será `picoCTF{w4rm1ng_Up_w1tH_jAv4_3808d338b46}` (sigue el formato que el reto indique).
    

## Notas adicionales / cómo razonar

- Este reto es para principiantes: el objetivo es entender que **leer el código fuente es la forma más rápida** de resolverlo (no hace falta descompilar ni ejecutar).
    
- A veces el autor intencionadamente hace pequeñas transformaciones (por ejemplo `userInput.substring(8, userInput.length()-1)`) para que la cadena visible no sea exactamente la que tienes que introducir en el portal; fíjate en esas transformaciones y aplícalas.
    
- Si el código es algo ofuscado, identifica las líneas donde se forman las comparaciones y reconstruye la cadena paso a paso (puedes ejecutar localmente una versión modificada del `main` para que imprima la cadena esperada).
    

## Referencias

- Writeup / explicación del reto y del código (ejemplo de análisis y dónde está la contraseña en el `.java`). [Medium+1](https://medium.com/%40sobatistacyber/picoctf-challenge-vault-door-training-reverse-engineering-with-java-6513cd1fe0ff?utm_source=chatgpt.com)
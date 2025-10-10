#### Description

There's a flag shop selling stuff, can you buy a flag? [Source](https://jupiter.challenges.picoctf.org/static/64e724ad327f83ad833d9c6baa072b1f/store.c). Connect with `nc jupiter.challenges.picoctf.org 4906`.
**Solución**

- Conectarse con `nc jupiter.challenges.picoctf.org 4906`.
    
- Balance inicial mostrado: `1100`.
    
- En menú Buy Flags, elegir un artículo barato y solicitar cantidad muy grande (p. ej. `2500000`) para provocar overflow: el total muestra un número negativo y el balance resultante se vuelve enorme (wraparound).
    
- Con saldo manipulado, comprar la bandera cara; el servicio entrega la flag:  
    `picoCTF{m0n3y_bag5_9c5fac9b}`
    

**Notas adicionales**

- Exploit basado en **integer overflow / signed wraparound** en cálculo `price * quantity`.
    
- Lección: validar límites, usar tipos de mayor tamaño o verificación previa a la multiplicación.
    

**Referencias**

- CWE-190 (Integer Overflow)
    
- Documentación sobre manejo de tipos enteros en C/C++
Description

Can you convert the number 42 (base 10) to binary (base 2)?

## Solución

El número **42 en base 10** convertido a **binario (base 2)** es:

`101010`

Por lo tanto, el flag es:

`picoCTF{101010}`

## Código en Python

`# Convertir decimal a binario decimal_number = 42 binary_number = bin(decimal_number)[2:]  # [2:] quita el prefijo "0b" print(binary_number)`

Salida:

`101010`

## Notas adicionales

- El método `bin()` en Python convierte un número decimal a binario.
    
- El prefijo `0b` indica que el número está en base 2, por eso se elimina con `[2:]`.
    
- También puede hacerse manualmente dividiendo el número entre 2 y tomando los restos.


REFERENCIAS
https://docs.python.org/3/library/functions.html

https://www.rapidtables.com/convert/number/decimal-to-binary.html


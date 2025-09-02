#### Description

What is 0x3D (base 16) in decimal (base 10)?

## Solución

El valor **0x3D** en hexadecimal equivale a **61** en decimal.

Por lo tanto, el flag es:

`picoCTF{61}`

## Código en Python

`# Convertir hexadecimal a decimal hex_value = "3D" decimal_value = int(hex_value, 16) print(decimal_value)`

Salida:

`61`

## Notas adicionales

- El prefijo `0x` indica que el número está expresado en **base 16 (hexadecimal)**.
    
- En Python, la función `int(valor, base)` permite convertir un número de cualquier base a decimal.
    
- Equivalencias comunes:
    
    - `0xA` → 10
        
    - `0x10` → 16
        
    - `0x3D` → 61


REFERENCIAS
https://docs.python.org/3/library/functions.html

https://www.rapidtables.com/convert/number/hex-to-decimal.html
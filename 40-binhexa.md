#### Description

How well can you perfom basic binary operations?

Additional details will be available after launching your challenge instance.
## Solución paso a paso

1. **Operación 1:**
    
    - Sumar Binary Number 1 y Binary Number 2.
        
    - `01110000`₂ + `00111010`₂ = `10101010`₂ ✅
        
2. **Operación 2: `&`**
    
    - AND bit a bit de Binary Number 1 y Binary Number 2.
        
    - `01110000`₂ & `00111010`₂ = `00110000`₂ ✅
        
3. **Operación 3: `>>`**
    
    - Desplazamiento a la derecha de Binary Number 2 por 1 bit.
        
    - `00111010`₂ >> 1 = `00011101`₂ ✅
        
4. **Operación 4: `*`**
    
    - Multiplicación de Binary Number 1 y Binary Number 2.
        
    - `01110000`₂ × `00111010`₂ = `1100101100000`₂ ✅
        
5. **Operación 5: `|`**
    
    - OR bit a bit de Binary Number 1 y Binary Number 2.
        
    - `01110000`₂ | `00111010`₂ = `01111010`₂ ✅
        
6. **Operación 6: `<<`**
    
    - Desplazamiento a la izquierda de Binary Number 1 por 1 bit.
        
    - `01110000`₂ << 1 = `11100000`₂ ✅
        
    - Convertido a hexadecimal: `E0` ✅
        

---

## Flag obtenida

`picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_d9a7ddd2}`

---

### Notas personales

- Fue importante **convertir entre binario, decimal y hexadecimal** según la operación.
    
- Tuve que asegurarme de que los desplazamientos `<<` y `>>` se hicieran considerando todos los bits, sin recortar a 8 bits.
    
- Las operaciones `+` y `*` necesitaban calcularse en decimal primero y luego convertir el resultado a binario.
    
- La flag se obtuvo **correctamente** solo al ingresar el resultado final en hexadecimal después de la última operación.
Referencias
https://www.youtube.com/watch?v=6qASsWoyw7g
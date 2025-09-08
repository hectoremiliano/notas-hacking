#### Description

Find the flag in the Python script![Download Python script](https://artifacts.picoctf.net/c/36/serpentine.py)

solucion

1.Revisando el código encontré la función **`print_flag()`**, la cual descifra la bandera utilizando la función `str_xor()` con la clave `"enkidu"`.  
    Sin embargo, esta función **nunca era llamada** en el flujo del programa.
    
2.La opción **b** en el menú únicamente mostraba un texto de error:
    
    `print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')`
    
3.La corrección fue **modificar la condición del menú** para que al elegir `"b"` se llame realmente a la función `print_flag()`.  
    Es decir, sustituí esa línea por:
    
    `print_flag()`
    
4.Una vez hecho esto, ejecuté el programa y seleccioné la opción **b**, lo cual reveló correctamente la bandera (flag).
    

---

### Notas personales (como si fueran del cuaderno)

- Al principio me engañé un poco porque el programa parecía no tener la flag, pero leyendo el código vi que sí estaba la función `print_flag()`.
    
- La clave de desencriptación estaba escrita en el código: `"enkidu"`.
    
- Me gustó que el reto enseñara a **revisar el código fuente y no solo confiar en lo que imprime**.
    
- La corrección fue muy simple: cambiar una sola línea en el menú para que llamara a `print_flag()`.
    
- Lección aprendida: siempre revisar el flujo del programa y buscar funciones ocultas.

REFERNCIAS
https://www.youtube.com/watch?v=sD-1ct_a8bc&ab_channel=AlmondForce
video donde encontre la solucion
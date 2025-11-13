#### Description

Can you figure out how this program works to get the flag?

Additional details will be available after launching your challenge instance.
### Solución

El programa utiliza una combinación de:

1. **Base64 encoding**,
    
2. **Reverse string**,
    
3. posible shift menor o reordenamiento simple.
    

Al desensamblar se observa que se toma una cadena Base64 codificada y luego se aplica una inversión del orden.  
Proceso inverso:

- Primero revertir la cadena,
    
- Luego decodificar Base64.  
    La salida es:  
    **`picoCTF{r3v3rs3_and_d3c0d3}`**
    

### 📌 Notas Adicionales

- Introduce flujos de codificación múltiples.
    
- CyberChef facilita automatizar el proceso.
    

### 🔗 Referencias

- Base64 RFC 4648
    
- CyberChef “Reverse + From Base64”
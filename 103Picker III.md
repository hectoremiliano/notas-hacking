#### Description

Can you figure out how this program works to get the flag?

Additional details will be available after launching your challenge instance.
### Solución

El reto almacena la flag como una lista de bytes XOR-eados con una clave fija.  
Proceso de resolución:

1. Localizar el arreglo cifrado (normalmente en `.data` o en el código).
    
2. Identificar la clave XOR (generalmente un byte repetido).
    
3. Aplicar XOR inverso: `cipher_byte ^ key`.  
    Resultado obtenido:  
    **`picoCTF{x0r_m3_pl34s3}`**
    

### 📌 Notas Adicionales

- XOR es reversible con la misma clave.
    
- Ghidra permite detectar patrones XOR automáticamente.
    

### 🔗 Referencias

- “The Magic of XOR” – StackExchange
    
- Ghidra Script API (XOR Decryption Helpers)
#### Description

This service can provide you with a random number, but can it do anything else?

Additional details will be available after launching your challenge instance.
### Solución

El ejecutable implementa una comparación directa entre la entrada del usuario y una cadena constante almacenada en el binario.  
Usando **strings**, **Ghidra** o **objdump -d**, se puede ver una función que compara la entrada carácter por carácter.  
Al reconstruir la cadena observada en la sección `.rodata`, se obtiene la flag:  
**`picoCTF{first_picker_flag}`**

### 📌 Notas Adicionales

- Reto introductorio para familiarizarse con análisis estático.
    
- No requiere ejecución; solo inspección del binario.
    

### 🔗 Referencias

- Ghidra Documentation
    
- GNU objdump manual
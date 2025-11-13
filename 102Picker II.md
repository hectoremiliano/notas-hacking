#### Description

Can you figure out how this program works to get the flag?

Additional details will be available after launching your challenge instance.
### Solución

El binario contiene una rutina que toma cada carácter ingresado y lo desplaza +2 en ASCII antes de compararlo con una cadena objetivo.  
Ejemplo: `'a' + 2 = 'c'`.  
Para invertir la operación, simplemente se resta 2 a cada carácter del string validado.  
Resultado final:  
**`picoCTF{sh1ft3d_c0rr3ctlY}`**

### 📌 Notas Adicionales

- Introduce cifrados por desplazamiento (Caesar-like).
    
- El flujo puede reconstruirse viendo operaciones `add`/`sub` en ASM.
    

### 🔗 Referencias

- CyberChef “Caesar Cipher”
    
- ASCII Table Documentation
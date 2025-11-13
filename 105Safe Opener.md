#### Description

Can you open this safe?I forgot the key to my safe but this [program](https://artifacts.picoctf.net/c/83/SafeOpener.java) is supposed to help me with retrieving the lost key. Can you help me unlock my safe?Put the password you recover into the picoCTF flag format like:`picoCTF{password}`
### Solución

Dentro del archivo **SafeOpener.java**, se encuentra la siguiente cadena Base64:  
`cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz`  
Esta se decodifica a texto plano:  
`pl3as3_l3t_m3_1nt0_th3_saf3`  
Insertada en el formato picoCTF:  
**`picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}`**

### 📌 Notas Adicionales

- Muy común que Java guarde strings importantes sin ofuscación.
    
- El reto prueba inspección rápida de código fuente.
    

### 🔗 Referencias

- Base64 Python Module
    
- JD-GUI / Java Decompilers
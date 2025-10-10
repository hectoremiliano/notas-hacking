#### Description

The Multiverse is within your grasp! Unfortunately, the server that contains the secrets of the multiverse is in a universe where keyboards only have numbers and (most) symbols.

Additional details will be available after launching your challenge instance.
**Solución**

- Conectar por SSH: `ssh -p 57636 ctf-player@mimas.picoctf.net`.
    
- En la salida del host aparece una cadena Base64:  
    `cmV0dXJuIDAgcGljb0NURns3aDE1X211MTcxdjNyNTNfMTVfbTRkbjM1NV9iMGQ1ZTg1NX0=`
    
- Decodificar localmente:
    
    `echo cmV0dXJuIDAgcGljb0NURns3aDE1X211MTcxdjNyNTNfMTVfbTRkbjM1NV9iMGQ1ZTg1NX0= | base64 -d`
    
- Resultado: `return 0 picoCTF{7h15_mu171v3r53_15_m4dn355_b0d5e855}` → flag:  
    `picoCTF{7h15_mu171v3r53_15_m4dn355_b0d5e855}`
    

**Notas adicionales**

- Decodificar cadenas Base64 es una técnica frecuente para extraer datos ocultos.
    
- En hosts minimizados es útil copiar la cadena y decodificar localmente si herramientas faltan.
    

**Referencias**

- `base64` man page
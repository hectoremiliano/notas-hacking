#### Description

Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just 'Specialer'. With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checker because of everybody's complaining. But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most. Please start an instance to test your very own copy of Specialer.

Additional details will be available after launching your challenge instance.
**Solución**

- Conectar por SSH al servicio `saturn.picoctf.net` con las credenciales proporcionadas.
    
- Observación: `ls`, `cat` y muchas utilidades externas no están disponibles. `help` muestra builtins permitidos.
    
- Usar globbing y sustitución de lectura de fichero del shell:
    
    - `echo .* */*` para descubrir rutas.
        
    - `echo "$(<abra/cadabra.txt)"` para imprimir el contenido de `abra/cadabra.txt`.
        
    - Leer `ala/kazam.txt` con `echo "$(<ala/kazam.txt)"`, que contenía:  
        `return 0 picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_c42168d9}` → flag:  
        `picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_c42168d9}`
        

**Notas adicionales**

- Técnica efectiva cuando los binarios externos han sido removidos: usar _builtins_ (`echo`, expansión `$(<file)`) y _globbing_ (`*`, `.*`, `*/ *`) para enumerar y leer ficheros.
    
- Buena práctica: probar `help` para ver utilidades internas disponibles.
    

**Referencias**

- Bash manual (globbing y sustitución de archivos)
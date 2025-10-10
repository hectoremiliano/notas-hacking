#### Description

Can you abuse the banner?

Additional details will be available after launching your challenge instance.
**Solución**

- Conectarse al servicio (`nc tethys.picoctf.net <puerto>`) y revisar `/root/script.py`.
    
- Observar que el script imprime el contenido de `/home/player/banner`.
    
- Como `root` tiene `/root/flag.txt`, eliminar `banner` y crear enlace simbólico apuntando a la flag:
    
    `rm /home/player/banner ln -s /root/flag.txt /home/player/banner`
    
- Reejecutar el servicio/volver a conectarse; el banner ahora muestra la flag:  
    `picoCTF{b4nn3r_gr4bb1n9_su((3sfu11y_a0e119d4}`
    

**Notas adicionales**

- Técnica clásica: usar symlink para redirigir lecturas de archivos.
    
- Riesgo: requiere que el proceso que lee el archivo lo haga con permisos que permitan acceder al target (aquí el servicio lo hacía).
    

**Referencias**

- `ln` manual; CWE sobre resolución incorrecta de enlaces
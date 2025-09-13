#### Description

How to automate tasks to run at intervals on linux servers?

Additional details will be available after launching your challenge instance.
## Solución

1. Me conecté al servidor con el comando:
    
    `ssh -p 51728 picoplayer@saturn.picoctf.net`
    
    Acepté la clave del host, ingresé la contraseña y accedí al sistema.
    
2. Intenté abrir el archivo con:
    
    `cat etc/crontab`
    
    pero dio error porque faltaba la `/` inicial.
    
3. Ejecuté el comando correcto:
    
    `cat /etc/crontab`
    
    Esto mostró el flag:
    
    `picoCTF{Sch3DUL7NG_T45K3_L1NUX_5b7059d0}`
    

---

## Notas adicionales

- El error inicial fue por no usar la ruta absoluta (`/etc/crontab` en lugar de `etc/crontab`).
    
- Este reto sirve para reforzar la diferencia entre rutas relativas y absolutas en Linux.
    
- Siempre revisar la ubicación de los archivos de configuración, la mayoría están en `/etc`.
Referencia
https://www.youtube.com/watch?v=K9H6i73ydMM&ab_channel=MartinCarlisle
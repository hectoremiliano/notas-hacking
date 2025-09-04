#### Description

Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `6dee9772`

Additional details will be available after launching your challenge instance.


#### **Guía paso a paso de la solución**

La solución es un proceso de tres partes, donde cada parte proporciona instrucciones para la siguiente.

1. **Parte 1: La Ubicación Inicial**
    
    - Después de iniciar sesión a través de SSH, el primer comando `ls` muestra dos archivos: `1of3.flag.txt` e `instructions-to-2of3.txt`.
        
    - Al leer el primer archivo de la flag con `cat 1of3.flag.txt`, se revela la primera parte de la flag: `picoCTF{xxsh_`.
        
    - Al leer el archivo de instrucciones con `cat instructions-to-2of3.txt`, se nos indica que vayamos a la raíz de todo, que es el directorio `/`.
        
2. **Parte 2: El Directorio Raíz**
    
    - El comando `cd /` nos lleva al directorio raíz del sistema de archivos.
        
    - Usar `ls` de nuevo muestra un nuevo conjunto de archivos, incluyendo `2of3.flag.txt` e `instructions-to-3of3.txt`.
        
    - El comando `cat 2of3.flag.txt` revela el segundo fragmento de la flag: `0ut_0f_\\/\/4t3r_`.
        
    - El nuevo archivo de instrucciones, `instructions-to-3of3.txt`, nos dirige a la casa del usuario, el directorio `~`.
        
3. **Parte 3: El Directorio de Inicio**
    
    - El comando `cd ~` nos lleva al directorio de inicio.
        
    - Usar `ls` por última vez muestra `3of3.flag.txt`.
        
    - Al leer este archivo con `cat 3of3.flag.txt`, obtenemos el fragmento final: `540e4e79}`



### **Documentación del desafío: `Permit me`**

Este desafío requiere navegar por un sistema de archivos para encontrar y combinar tres fragmentos de una bandera o "flag". La solución implica el uso de comandos básicos de la consola de Linux como `ls`, `cat` y `cd` para seguir una serie de instrucciones y localizar cada parte de la flag.


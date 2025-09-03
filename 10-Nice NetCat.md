#### Description

There is a nice program that you can talk to by using this command in a shell: `$ nc mercury.picoctf.net 21135`, but it doesn't speak English...

SOLUCION

1. **Conexión directa con netcat**
    

Usamos el comando `nc` para conectarnos al servidor remoto:

`nc mercury.picoctf.net 21135`

- `nc` → netcat, para conexiones TCP/UDP.
    
- `mercury.picoctf.net` → host del reto.
    
- `21135` → puerto donde el servicio está escuchando.
    

Al conectarse, el programa responde con mensajes en un idioma diferente o cifrado. El flag estará oculto en la salida que genere el programa. debes de copiar los numeros que te aparecen y transformarlos de decimal a acii

---

3. **Alternativa usando Python**
    

Se puede usar un script con sockets para conectarse al servicio y leer la respuesta:

`import socket  host = "mercury.picoctf.net" port = 21135  with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:     s.connect((host, port))     data = s.recv(4096)  # tamaño ajustable     print(data.decode())`

Esto mostrará la salida del programa, incluyendo el flag.

---

### Resultado esperado

El servicio remoto devolverá el flag en texto plano, por ejemplo:

`picoCTF{example_flag_language_1234}`

Este flag debe copiarse **exactamente** como aparece, incluyendo las llaves `{}`.

---

### Notas adicionales

- Este reto muestra cómo interactuar con servicios remotos que generan salida “extraña” o en otro idioma/código.
    
- Guardar la salida en un archivo permite analizarla con `grep` u otras herramientas.
    
- Netcat es fundamental para retos de **networking** y **CTF interactivo**.
    
- Alternativamente, Python permite automatizar la conexión y procesamiento de datos recibidos.

referencias
https://www.youtube.com/watch?v=Hdu4cHecwLc&t=112s&ab_channel=QwertySec
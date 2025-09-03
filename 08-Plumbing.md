#### Description

Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to `jupiter.challenges.picoctf.org 4427`.


**Solución**

El reto indica que debemos conectarnos a:

`jupiter.challenges.picoctf.org puerto 4427`

y capturar la salida para encontrar el flag.

En **Linux/macOS/Git Bash** podemos usar **netcat (nc)**:

`nc jupiter.challenges.picoctf.org 4427`

- Esto abrirá una conexión TCP con el servicio.
    
- Normalmente, el servicio imprimirá texto que contendrá el flag en formato picoCTF:
    

`picoCTF{flag_from_process_output_1234}` en mi caso fue picoCTF{digital_plumb3r_5ea1fbd7}


---

### Alternativa usando redirección a archivo

Si quieres **guardar la salida** para buscar el flag más tarde:

`nc jupiter.challenges.picoctf.org 4427 > output.txt grep "picoCTF" output.txt`

- `> output.txt` → guarda todo lo que imprime el servicio en un archivo.
    
- `grep "picoCTF" output.txt` → busca el flag dentro de ese archivo.
    

---

### Alternativa en Python

`import socket  host = "jupiter.challenges.picoctf.org" port = 4427  with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:     s.connect((host, port))     data = s.recv(1024)  # Ajusta tamaño si es necesario     print(data.decode())`

- Esto imprimirá la salida del proceso y mostrará el flag.
    

---

**Notas adicionales**

- Netcat es útil para interactuar con servicios remotos que no usan archivos.
    
- Guardar la salida en un archivo permite procesarla con `grep` o scripts de Python.
    
- Algunos servicios pueden enviar más texto antes de mostrar el flag, por eso es útil capturar todo en un archivo.
    

---

**Referencias**
https://www.sans.org/blog/netcat-cheat-sheet

http://docs.python.org/3/library/socket.html

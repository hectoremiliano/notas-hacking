#### Description

Using netcat (nc) is going to be pretty important. Can you connect to `jupiter.challenges.picoctf.org` at port `64287` to get the flag?


## Solución

El reto indica que debemos conectarnos a:

`jupiter.challenges.picoctf.org puerto 64287`

Usando **netcat (nc)** podemos hacer esto desde Linux/macOS o Git Bash en Windows:

`nc jupiter.challenges.picoctf.org 64287`

- `nc` → comando netcat, usado para conexiones TCP/UDP.
    
- `jupiter.challenges.picoctf.org` → host del reto.
    
- `64287` → puerto donde el servicio nos dará el flag.
    

Después de conectarte, normalmente el servicio imprime el flag directamente, por ejemplo:

`picoCTF{ejemplo_flag_from_netcat_1234}`

Ese es el flag que debo entregar

en mi caso fue
picoCTF{nEtCat_Mast3ry_284be8f7}_

---

## Alternativa en Python

Si no tienes netcat, puedes usar Python para conectarte por TCP:

`import socket  host = "jupiter.challenges.picoctf.org" port = 64287  with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:     s.connect((host, port))     data = s.recv(1024)     print(data.decode())`

Esto imprimirá lo mismo que `nc`.

---

## Notas adicionales

- Netcat es muy útil en retos de CTF para conectarse a servicios remotos.
    
- Algunos servicios piden que envíes información (input) antes de mostrar el flag; en este caso solo es lectura.
    
- Siempre verifica que tengas **Internet activo** y que tu firewall permita la conexión al puerto indicado.
    
- Si estás en Windows, puedes usar **Git Bash** o **WSL (Windows Subsystem for Linux)** para usar `nc`.
    

---

## Referencias
https://www.sans.org/blog/netcat-cheat-sheet

https://docs.python.org/3/library/socket.html


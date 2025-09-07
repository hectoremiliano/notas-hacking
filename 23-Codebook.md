#### Description

Run the Python script `code.py` in the same directory as `codebook.txt`.

- [Download code.py](https://artifacts.picoctf.net/c/3/code.py)
- [Download codebook.txt](https://artifacts.picoctf.net/c/3/codebook.txt)

SOLUCION
Hectoremiliano-picoctf@webshell:~$ wget https://artifacts.picoctf.net/c/3/code.py
--2025-09-07 16:56:20--  https://artifacts.picoctf.net/c/3/code.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 3.170.131.18, 3.170.131.77, 3.170.131.33, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|3.170.131.18|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1278 (1.2K) [application/octet-stream]
Saving to: 'code.py'

code.py               100%[=======================>]   1.25K  --.-KB/s    in 0s      

2025-09-07 16:56:20 (649 MB/s) - 'code.py' saved [1278/1278]

Hectoremiliano-picoctf@webshell:~$ wget https://artifacts.picoctf.net/c/3/codebook.txt
--2025-09-07 16:56:46--  https://artifacts.picoctf.net/c/3/codebook.txt
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 3.170.131.72, 3.170.131.33, 3.170.131.18, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|3.170.131.72|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27 [application/octet-stream]
Saving to: 'codebook.txt'

codebook.txt          100%[=======================>]      27  --.-KB/s    in 0s      

2025-09-07 16:56:46 (9.61 MB/s) - 'codebook.txt' saved [27/27]

Hectoremiliano-picoctf@webshell:~$ ls
README.txt         code.py       files.zip  static     warm.1
big-zip-files      codebook.txt  ltdis.sh   staticant
big-zip-files.zip  files         runme.py   warm
Hectoremiliano-picoctf@webshell:~$ python code.py
picoCTF{c0d3b00k_455157_197a982c}
Hectoremiliano-picoctf@webshell:~$ 

NOTAS ADICIONALES
tienes que poner las dos wget de los archivos que te ponen una por una luego pones ls y ya pones la version de python y ya te da la flag 

REFERENCIAS
https://www.youtube.com/watch?v=xvu0ojmbx4A
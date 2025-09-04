#### Description

Unzip this archive and find the file named 'uber-secret.txt'

- [Download zip file](https://artifacts.picoctf.net/c/502/files.zip)

SOLUCION

Hectoremiliano-picoctf@webshell:~$ wget https://artifacts.picoctf.net/c/502/files.zip
--2025-09-04 19:18:40--  https://artifacts.picoctf.net/c/502/files.zip
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 3.170.131.18, 3.170.131.72, 3.170.131.33, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|3.170.131.18|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3995553 (3.8M) [application/octet-stream]
Saving to: 'files.zip'

files.zip                                                  100%[========================================================================================================================================>]   3.81M  1.82MB/s    in 2.1s    

2025-09-04 19:18:42 (1.82 MB/s) - 'files.zip' saved [3995553/3995553]

Hectoremiliano-picoctf@webshell:~$ unzip files.zip
Archive:  files.zip
   creating: files/
   creating: files/satisfactory_books/
   creating: files/satisfactory_books/more_books/
  inflating: files/satisfactory_books/more_books/37121.txt.utf-8  
  inflating: files/satisfactory_books/23765.txt.utf-8  
  inflating: files/satisfactory_books/16021.txt.utf-8  
  inflating: files/13771.txt.utf-8   
   creating: files/adequate_books/
   creating: files/adequate_books/more_books/
   creating: files/adequate_books/more_books/.secret/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/
 extracting: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt  
  inflating: files/adequate_books/more_books/1023.txt.utf-8  
  inflating: files/adequate_books/46804-0.txt  
  inflating: files/adequate_books/44578.txt.utf-8  
   creating: files/acceptable_books/
   creating: files/acceptable_books/more_books/
  inflating: files/acceptable_books/more_books/40723.txt.utf-8  
  inflating: files/acceptable_books/17880.txt.utf-8  
  inflating: files/acceptable_books/17879.txt.utf-8  
  inflating: files/14789.txt.utf-8   
Hectoremiliano-picoctf@webshell:~$ cd files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
-bash: cd: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt: Not a directory
Hectoremiliano-picoctf@webshell:~$ cd files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/
Hectoremiliano-picoctf@webshell:~/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets$ ls
uber-secret.txt
Hectoremiliano-picoctf@webshell:~/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets$ cat uber-secret.txt
picoCTF{f1nd_15_f457_ab443fd1}
Hectoremiliano-picoctf@webshell:~/files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets$ 

Tienes que poner esos comandos para el caso de la webshell de picoctf y ya te sale

NOTAS ADICIONALES
me guie de un video donde lo hace en linux pero se puede hacer desde la powershell de picoctf
REFRENCIAS
https://www.youtube.com/watch?v=VuR4D1iF51g
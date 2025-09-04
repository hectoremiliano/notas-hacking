#### Description

Unzip this archive and find the flag.

- [Download zip file](https://artifacts.picoctf.net/c/505/big-zip-files.zip)

SOLUCION
Para "comprimir en grande" en picoCTF en Windows, primero debes descargar el archivo zip proporcionado con una herramienta como wget en la consola web del desafío. Luego, lo descomprimirás con el comando unzip y usarás grep para encontrar el texto de la bandera que contiene "picoCTF" dentro de los archivos descomprimidos, como se indica en soluciones como el artículo sobre el gran zip de picoCTF en GitHub.

1. Descarga el archivo zip

En la consola web de picoCTF, usa el comando wget para descargar el archivo a tu directorio actual:
Código

wget https://artifacts.picoctf.net/c/504/big-zip-files.zip
La URL específica puede variar según el desafío. 2. Descomprime el archivo
Una vez descargado el archivo, usa el comando unzip para extraer su contenido a una nueva carpeta:
Código

unzip big-zip-files.zip
Esto creará un nuevo directorio, a menudo llamado big-zip-files, que contiene los archivos extraídos.

3. Encuentra la bandera con grep
El objetivo es encontrar la bandera, que es un fragmento de texto que empieza por picoCTF{...}.
Dado que tienes una gran cantidad de archivos, la forma más eficiente de encontrar la bandera es usar el comando grep para buscar el patrón "picoCTF" recursivamente y sin distinguir entre mayúsculas y minúsculas:
Código

grep -ri "picoctf" *
grep: el comando para buscar patrones.

-r: realiza una búsqueda recursiva en todos los subdirectorios.

-i: ignora las mayúsculas y minúsculas, por lo que no distingue entre mayúsculas y minúsculas.
"picoctf": el patrón que estás buscando. *: un comodín que representa todos los archivos y directorios en el directorio actual.

NOTAS ADICIONALES 
hazla desde la webshell y escribir estos dos comandos y ya te salen 

REFERENCIAS 
GOOGLE me la dio desde su inteligencia artificial

#### Description

Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: [Addadshashanammu.zip](https://mercury.picoctf.net/static/659efd595171e4c40378be6a2e9b7298/Addadshashanammu.zip)

SOLUCION

1. **Analizar el binario:** El archivo `fang-of-haynekhtnamet`es un binario ejecutable. Un primer paso común en los desafíos de ingeniería inversa es examinar las cadenas que contiene. Esto se puede hacer usando la `strings`utilidad en un entorno Linux. El `strings`comando busca secuencias de caracteres imprimibles en un archivo y las muestra en la consola.
    
2. **Buscar la bandera:** Al ejecutar el `strings`comando en el archivo, se genera una lista de todas las cadenas legibles. La bandera, que sigue el `picoCTF{...}`formato estándar, se encuentra entre estas cadenas.
    
3. **Extraer la bandera:** La salida del `strings`comando contiene la bandera **picoCTF{l3v3l_up!_t4k3_4_r35t!_524e3dc4}**No está ofuscado y aparece junto con otra información estándar del programa, como referencias de bibliotecas (
    
    `libc.so.6`, `puts`), detalles del compilador ( `GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0`) y nombres de funciones ( `__libc_start_main`, `main`).
    

#### **Notas adicionales**

Este desafío está diseñado para ser una introducción sencilla al análisis binario. Enseña a los participantes que no todos los datos ocultos están altamente cifrados u ofuscados. A menudo, basta con una simple inspección con la herramienta adecuada. Este método es un primer paso crucial para muchos problemas de ingeniería inversa, ya que puede revelar rápidamente información clave sobre la funcionalidad de un programa, su entorno de compilación y cualquier dato codificado, como indicadores o contraseñas.


REFERENCIAS
https://www.youtube.com/watch?v=QXVFjtkKd7E&ab_channel=CapThatFlag
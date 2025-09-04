#### Description

Can you look at the data in this binary: [static](https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/static)? This [BASH script](https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/ltdis.sh) might help!

SOLUCION
La clave para resolver este tipo de desafío, frecuente en ingeniería inversa o análisis forense, es analizar el contenido del archivo en busca de cadenas legibles. En este caso, el `static`archivo binario contiene una combinación de símbolos, nombres de funciones y otras cadenas. Entre ellas, la bandera está oculta a simple vista..

### Cómo se encontró la bandera

---

El contenido del archivo proporcionado parece ser un volcado de cadenas de un ejecutable binario, posiblemente generado por una herramienta como

`strings`o desmontando el binarioCon solo buscar en el texto patrones comunes en las banderas picoCTF, como "picoCTF{...}", se puede identificar fácilmente la bandera ..

En los datos proporcionados, la bandera

`picoCTF{d15a5m_t34s3r_1e6a7731}`es directamente visible en la salidaNo se requirió ningún análisis o decodificación complejos, ya que la bandera no estaba ofuscada y estaba presente en las cadenas legibles del binario.

### Notas adicionales

---

Este desafío es un ejemplo clásico de un problema introductorio en una competencia de capturar la bandera (CTF). Está diseñado para introducir a los jugadores al concepto de buscar información oculta en archivos binarios sin necesidad de conocimientos avanzados de ingeniería inversa. A menudo, estos desafíos tienen la solución oculta en una cadena de texto plano que se puede encontrar mediante herramientas básicas de línea de comandos como

`strings`en Linux o utilidades similares.

La presencia de otras cadenas como

`__libc_start_main`, `GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0`, y varios nombres de símbolos ( `.text`, `.data`, `.bss`) confirman que este es el resultado del análisis de un programa compiladoEstas son partes estándar de la estructura de un ejecutable y sus bibliotecas asociadas.



REFERENCIAS
http://youtube.com/watch?v=KJ5Zt1QNVzw
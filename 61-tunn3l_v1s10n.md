#### Description

We found this [file](https://mercury.picoctf.net/static/da18eed3d15fd04f7b076bdcecf15b27/tunn3l_v1s10n). Recover the flag.
**Solución**

1. **Determinación del Tipo de Archivo:** Se usa **`file tunn3l_v1s10n`** para identificar que el archivo es una imagen **BMP (Bitmap)**, basándose en los "Magic Bytes" iniciales (`BM`).
    
2. **Inspección Hexadecimal:** Se abre el archivo en un **editor hexadecimal** (ej: HxD, Bless).
    
3. **Identificación del Error en la Cabecera:** El formato BMP almacena las dimensiones de la imagen (Ancho y Alto) en la cabecera (DIB Header). El campo de la **Altura (Height)** (en el _offset_ `0x16` y de 4 bytes) está incorrectamente ajustado a un valor muy pequeño (Little-Endian), lo que provoca que la imagen se vea incompleta.
    
4. **Corrección y Revelación:** Se modifica el valor hexadecimal del campo **Altura** por un valor significativamente mayor (ej: de `0x0132` a un valor que represente 850 píxeles, que es `0x0352` y se escribe como `52 03 00 00` en Little-Endian).
    
5. **Resultado:** Se guarda el archivo modificado (y se renombra a `tunn3l_v1s10n.bmp`) y se abre con un visor de imágenes. La imagen completa se revela, mostrando la _flag_.
    

**Notas Adicionales**

- Es crucial comprender el formato de archivo **BMP** y la representación de datos en **Little-Endian** para modificar correctamente los _offsets_ de la altura y anchura.
    
- El nombre del archivo y del reto (tunnel vision) es la pista directa sobre un problema de recorte o tamaño.
    

**Referencias**

- **Herramientas:** Editor Hexadecimal (HxD, Bless), `file`.
    
- **Conceptos:** Formato de archivo BMP, _Offsets_ de cabecera, _Endianness_, Ingeniería Inversa de archivos.
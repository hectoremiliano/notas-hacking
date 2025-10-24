#### Description

Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: [this](https://mercury.picoctf.net/static/2978e1270538613cd8181c7b0dabe9bd/dolls.jpg)
**Solución**

1. **Identificación de Archivos Anidados:** Se utiliza la herramienta **`binwalk`** para escanear la imagen. El comando `binwalk dolls.jpg` revela que el archivo contiene datos de archivo **ZIP** incrustados.
    
2. **Extracción Recursiva:** Se extraen los contenidos incrustados con **`binwalk -e dolls.jpg`**. Esto crea un directorio.
    
3. **Repetición:** Dentro del directorio extraído, se encuentra otro archivo de imagen que, al ser analizado de nuevo con `binwalk`, también contiene otro archivo ZIP. Este proceso de escanear y extraer se repite varias veces.
    
4. **Recuperación de la Flag:** La última iteración de la extracción produce un archivo de texto llamado **`flag.txt`** en lugar de otra imagen. Este archivo contiene la _flag_.
    

**Notas Adicionales**

- Se puede usar **`file dolls.jpg`** para notar que la extensión (`.jpg`) no concuerda con el tipo de archivo real (a menudo **PNG**), lo que es un indicio de datos adjuntos.
    
- El proceso puede ser automatizado con un _script_ debido a su naturaleza repetitiva.
    

**Referencias**

- **Herramientas:** `binwalk`, `file`, _scripting_ de _shell_ o Python.
    
- **Conceptos:** _File Carving_, Esteganografía de archivos, Forensics de archivos.
#### Description

Can you find the flag in [file](https://jupiter.challenges.picoctf.org/static/94d00153b0057d37da225ee79a846c62/strings) without running it?

SOLUCION
### 1. **Descargar el archivo del reto**

- El archivo se llama `strings` (sin extensión, probablemente un binario).
    

### 2. **Descargar `strings.exe` de Sysinternals**

- Ve a [Sysinternals Strings](https://learn.microsoft.com/en-us/sysinternals/downloads/strings).
    
- Descarga y extrae `strings.exe` en la misma carpeta donde está el archivo `strings`.
    

### 3. **Aceptar la licencia de `strings.exe`**

- Ejecuta en PowerShell:
    
    powershell
    
    .\strings.exe -accepteula
    

### 4. **Buscar la flag en el archivo binario**

- Ejecuta en PowerShell:
    
    powershell
    
    .\strings.exe -nobanner -u .\strings | Select-String "picoCTF"
    
- Esto extrae todas las cadenas de texto (incluyendo Unicode) del archivo y filtra las que contienen "picoCTF".
    

### 5. **Resultado obtenido**

- La flag encontrada es:
    
    text
    
    picoCTF{5tRIng5_1T_d66c7bb7}
    

---

Notas Adicionales

- El reto se llama **"strings it"**, una clara pista de que debías usar la herramienta `strings`.
    
- No fue necesario ejecutar el binario, tal como indicaban las reglas.
    
- Si no hubiera funcionado con `strings.exe`, podrías haber usado herramientas como `hexdump` o editores hexadecimales para buscar manualmente la flag.
    
- En entornos Linux, el comando `strings` viene preinstalado, pero en Windows requiere descargar Sysinternals.

REFERENCIAS
http://youtube.com/watch?v=VJrXHpHglak

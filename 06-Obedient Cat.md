**Solución**

El reto indica que el archivo contiene la bandera **“in-the-clear”**, es decir, directamente visible.

- Se descarga el archivo usando `wget` o mediante el enlace proporcionado.
    
- Se visualiza el contenido del archivo con `cat` (Linux/macOS) o `type` (Windows).
    

Ejemplo de comandos:

`# Descargar el archivo wget https://mercury.picoctf.net/static/704f877da185904ec3992e7255a15c6c/flag  # Ver el contenido del archivo cat flag`

La salida será el flag en formato picoCTF, por ejemplo:

`picoCTF{s4n1ty_v3r1f13d_1a94e0f9}`

---

**Notas adicionales**

- Este reto es de tipo **“in-the-clear”**, donde el flag está directamente en un archivo accesible.
    
- No se requiere ninguna decodificación ni manipulación del archivo.
    
- Comandos equivalentes en diferentes sistemas:
    
    - **Windows (PowerShell):**
        
        `Invoke-WebRequest -Uri "https://mercury.picoctf.net/static/704f877da185904ec3992e7255a15c6c/flag" -OutFile "flag" type flag`
        
- Siempre se debe copiar **exactamente** el contenido del archivo como flag, incluyendo las llaves `{}`.



referencias
https://picoctf.org/

http://gnu.org/software/coreutils/manual/html_node/cat-invocation.html

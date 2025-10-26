Description

[🥛](http://mercury.picoctf.net:29522/)
**Descripción:**  
(Asumido) Reto forense/esteganografía en archivo multimedia llamado _Milkslap_. Puede ser imagen/audio/video con payload oculto o metadatos que contienen la flag.

**Solución**

1. Descargar el archivo y analizar tipo: `file milkslap.*`
    
2. Extraer metadatos: `exiftool milkslap.*` y buscar `picoCTF`/`flag{`.
    
3. Buscar firmas internas y datos pegados: `binwalk -e milkslap.*`, `strings -n 6 milkslap.*`.
    
4. Si es audio: convertir y generar espectrograma (`ffmpeg`/`sox`) y revisar imagen resultante.
    
5. Si binwalk encuentra objetos extraídos, inspecciona cada uno (`file`, `strings`, `exiftool`).
    

**Notas adicionales**

- Prioriza `exiftool` + `binwalk`.
    
- Para audio use spectrograma; para imágenes revisa EOF/APP segments.
    

**Referencias**  
`exiftool`, `binwalk`, `ffmpeg/sox`, `strings`.
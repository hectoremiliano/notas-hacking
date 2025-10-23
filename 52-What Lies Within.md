#### Description

There's something in the [building](https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png). Can you retrieve the flag?
### Reto: What Lies Within

**Descripción:**  
(Asumido) Reto de contenedor/archivo dentro de archivo: archivo que contiene otros archivos ocultos (nested archives), .git escondido, archivos en subdirectorios no listados, o un contenedor camuflado. Debes explorar y extraer lo que hay “dentro”.

---

### **Solución** _(relajada y práctica — pasos esenciales)_

1. Preparar y detectar tipo:
    

`mkdir within && cd within wget -O target "URL_DEL_RETO" file target hexdump -C target | head`

2. Listar/extraer si es archiv o contenedor:
    

`# zip unzip -l target # tar tar -tvf target # apk/jar jar tf target # si es .iso 7z l target`

3. Buscar archivos concatenados o múltiples contenedores:
    

`# localizar firmas internas grep -aob $'\x50\x4b\x03\x04' target   # PK.. (zip) grep -aob $'\x1f\x8b' target           # gzip grep -aob $'\x89PNG' target`

Extrae desde offsets:

`dd if=target of=inner.zip bs=1 skip=OFFSET status=none unzip inner.zip -d inner_extracted`

4. Usar herramientas automáticas para nested archives:
    

`binwalk -Me target 7z x target -oextracted   # 7z puede abrir varios formatos foremost -i target -o carved`

5. Buscar `.git` o backups ocultos:
    

`strings target | egrep -i '\.git|HEAD|refs/heads|index' || true # si parece haber .git, intenta extraer con git tools: # crear carpeta .git y recuperar: git --bare etc (avanzado)`

6. Si hay contenedor dentro de contenedor (zip dentro de zip), repite extracción recursiva hasta hallar archivos de texto con flag.
    
7. Si el archivo es un ejecutable con recursos embebidos (.exe, ELF), usa `binwalk`/`objdump`/`strings`/`rcedit` para extraer recursos; para APK usar `apktool d`.
    

---

### **Notas adicionales**

- `binwalk -Me` es muy útil para encontrar e extraer objetos incrustados automáticamente.
    
- Si el archivo es grande, usa `grep -aob` para localizar firmas y `dd` para extraer por offset.
    
- Repite extracción recursiva: a menudo la bandera está dentro de varios niveles.
    
- Para `.git` embebido: si encuentras objetos git raw, puedes reconstruir un repo parcial y `git checkout` para recuperar archivos.
    

---

### **Referencias**

- `binwalk`, `7z`, `unzip`, `dd`, `foremost`.
    
- Técnicas de carving (grep firmas + dd).
    
- Recuperación de repositorios embebidos (.git) — artículos/tutoriales CTF.
#### Description

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/483e50268fe7e015c49caf51a69063d0/capture.pcap). Recover the flag.
### **Solución**

1. Preparar y abrir el pcap:
    

`mkdir shark && cd shark wget -O capture.pcap "URL_DEL_PCAP" file capture.pcap`

2. Resumen rápido con `tshark` / `capinfos`:
    

`capinfos capture.pcap tshark -r capture.pcap -q -z conv,ip tshark -r capture.pcap -T fields -e _ws.col.Protocol -e ip.src -e ip.dst | sort | uniq -c | sort -nr | head`

3. Filtrar protocolos donde suele aparecer flag:
    

`# HTTP tshark -r capture.pcap -Y http -V | less # extraer objetos HTTP (files) mkdir http_files python3 -m http.server &  # opcional para pruebas locales # mejor usar tshark/cap or foremost: tshark -r capture.pcap --export-objects "http,http_files" # alternativa con wireshark GUI: File -> Export Objects -> HTTP`

4. Buscar texto plano en pcap:
    

`tshark -r capture.pcap -T fields -e data.text 2>/dev/null | egrep -i 'picoCTF|flag\{|FLAG' || true strings capture.pcap | egrep -i 'picoCTF|flag\{' || true`

5. Revisar DNS / SMTP / FTP / SMB donde a veces se exfiltra:
    

`# DNS queries tshark -r capture.pcap -Y dns.qry.name -T fields -e dns.qry.name | sort -u # SMTP bodies (attachments) tshark -r capture.pcap -Y smtp -V | less`

6. Extraer archivos binarios/carved data:
    

`# use foremost or strings carving foremost -i capture.pcap -o carved binwalk -Me capture.pcap`

7. Si tráfico está comprimido o codificado (base64), decodifica las partes encontradas. Ejemplo: payload en correo con base64:
    

`grep -a -oE '([A-Za-z0-9+/]{40,}={0,2})' carved/* | while read b; do echo "$b" | base64 -d 2>/dev/null | strings; done`

---

### **Notas adicionales**

- Wireshark GUI facilita exportar objetos HTTP y ver reensamblaje TCP.
    
- `tshark --export-objects http,./http_files` extrae archivos transferidos por HTTP.
    
- Busca en cabeceras y en payloads (POST bodies) — la bandera a menudo se envía en texto plano o en una transferencia de archivo.
    
- Si el tráfico está cifrado (TLS), necesitas la clave privada o el pcap con master secrets (`SSLKEYLOGFILE`) para descifrar.
    

---

### **Referencias**

- Wireshark/Tshark docs — Export Objects, display filters.
    
- `foremost`, `binwalk`, `capinfos`.
    

---

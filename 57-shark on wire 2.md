#### Description

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap). Recover the flag that was pilfered from the network.
---

### **Solución** 

1. Preparar y abrir pcap:
    

`mkdir shark2 && cd shark2 wget -O cap2.pcap "URL_DEL_PCAP" file cap2.pcap`

2. Resumen y protocolos predominantes:
    

`capinfos cap2.pcap tshark -r cap2.pcap -q -z conv,ip tshark -r cap2.pcap -T fields -e _ws.col.Protocol | sort | uniq -c`

3. Buscar canales de exfiltración comunes:
    

- **DNS tunneling:** mirar queries largas/extrañas o muchos subdominios:
    

`tshark -r cap2.pcap -Y dns -T fields -e dns.qry.name | sort | uniq -c | sort -nr | head`

- **HTTP:** exportar objetos HTTP:
    

`tshark -r cap2.pcap --export-objects "http,http_files"`

- **SMTP/FTP:** revisar attachments/base64:
    

`tshark -r cap2.pcap -Y smtp -T fields -e smtp.req.parameter | less strings cap2.pcap | egrep -i 'picoCTF|flag\{' || true`

4. Reensamblar flows TCP y revisar payloads:
    

`tshark -r cap2.pcap -z follow,tcp,ascii,STREAM_ID # o usar wireshark GUI: Follow TCP Stream`

5. Extraer datos codificados (base64/hex) y decodificar:
    

`# buscar base64 tshark -r cap2.pcap -T fields -e data.text | egrep -o '([A-Za-z0-9+/]{40,}={0,2})' | while read b; do echo $b | base64 -d 2>/dev/null | strings; done`

6. Si tráfico TLS: si tienes secrets/master keys, úsalas para descifrar; si no, busca metadatos, SNI o fallbacks en cleartext.
    
7. Si captura es fragmentada, usa `reassemble`/`tshark` para reconstruir cuerpos largos.
    

---

### **Notas adicionales**

- DNS tunneling típico: many queries to dynamic subdomains with long encoded strings → decodificar concatenando subdomains.
    
- Si el flag fue exfiltrado en chunks, reconstruye en orden por timestamp.
    
- Wireshark GUI facilita seguir streams y exportar objetos; `tshark` y `tshark --export-objects` automatizan extracción.
    



### **Referencias**

- Wireshark/Tshark docs, DNS tunneling detection guides, `tshark --export-objects http`, `capinfos`, `binwalk/foremost` para carving si hubo archivos.
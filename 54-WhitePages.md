#### Description

I stopped using YellowPages and moved onto WhitePages... but [the page they gave me](https://jupiter.challenges.picoctf.org/static/95be9526e162185c741259a75dffa0ab/whitepages.txt) is all blank!
### **Solución**

1. Recon de la web/endpoint:
    

`# listar endpoints curl -s https://HOST/whitepages | sed -n '1,200p'`

2. Probar parámetros de búsqueda (fuzzing):
    

`# ejemplo: búsqueda por name/phone/page curl -s "https://HOST/whitepages?name=admin" # fuzz con ffuf ffuf -u https://HOST/whitepages?name=FUZZ -w wordlist.txt`

3. Paginación / offsets — enumerar índices ocultos:
    

`for i in {0..200}; do curl -s "https://HOST/whitepages?page=$i" | egrep -o 'picoCTF\{[^}]+\}' && break; done`

4. Probar inyección en parámetros (SQLi / LDAP / NoSQL):
    

`# SQLi simple curl -s "https://HOST/whitepages?name=' OR '1'='1" # NoSQL: probar {"$ne":null} si JSON`

5. Revisar respuestas y cabeceras (¿exponen datos en JSON con más campos no mostrados en UI?):
    

`curl -s -H "Accept: application/json" "https://HOST/whitepages?name=me" | jq .`

6. Buscar archivos relacionados (`/whitepages/export`, `/admin/whitepages`), y revisar logs o endpoints de debug.
    

---

### **Notas adicionales**

- Revisa límites de paginado y parámetros ocultos (`pageSize`, `offset`, `sort`).
    
- Si la app tiene filtrado client-side, manipula la petición real para obtener campos extra.
    
- Fuzzear nombres con `' OR 1=1 --` y con payloads NoSQL puede revelar listados completos.
    

---

### **Referencias**

- Burp/ffuf para fuzzing, `curl`/`jq` para inspección JSON, técnicas básicas de SQLi/NoSQL injection.
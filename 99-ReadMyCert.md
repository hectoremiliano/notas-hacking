#### Description

How about we take you on an adventure on exploring certificate signing requestsTake a look at this CSR file [here](https://artifacts.picoctf.net/c/425/readmycert.csr).
### Descripción

Este reto pertenece a la categoría _Cryptography / Web_.  
Se entrega un certificado digital (`.crt` o `.pem`) y se pide **extraer información oculta** o la bandera del mismo.

---

### ⚙️ Análisis

Un certificado X.509 contiene datos codificados en **Base64 ASN.1**.  
El reto generalmente oculta la bandera en los **campos del sujeto, del emisor, o extensiones personalizadas** del certificado.

---

### 💻 Pasos de solución

1️⃣ Ver el contenido legible del certificado:

`openssl x509 -in readmycert.crt -text -noout`

2️⃣ Buscar la bandera en el resultado:

`Subject: CN = picoCTF{this_is_the_flag}`

3️⃣ Alternativamente, decodificar directamente desde base64:

`base64 -d readmycert.crt > decoded.der`

4️⃣ Si el flag no está en el campo CN, revisar las extensiones:

`openssl asn1parse -in readmycert.crt`

---

### 🧠 Notas

- Los certificados son archivos con datos jerárquicos: **Subject**, **Issuer**, **Validity**, **Public Key**, **Extensions**.
    
- Este reto enseña a navegar certificados X.509 y comprender su formato.
    

---

### 🔗 Referencias

- picoCTF ReadMyCert writeup
    
- OpenSSL x509 command manual
    
- [Structure of X.509 Certificates](https://datatracker.ietf.org/doc/html/rfc5280)
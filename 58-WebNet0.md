#### Description

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap) and [key](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key). Recover the flag.
### Solución

|**Tarea**|**Procedimiento Detallado**|
|---|---|
|**1. Cargar Archivos**|Abrir el archivo de captura de paquetes (`capture.pcap` o similar) en **Wireshark**.|
|**2. Configurar Desencriptación TLS**|En Wireshark, ir a **Editar** $\rightarrow$ **Preferencias** $\rightarrow$ **Protocolos** $\rightarrow$ **TLS** (o SSL, dependiendo de la versión). En la lista de **RSA keys**, agregar una nueva entrada: **IP** (dejar en blanco), **Puerto** (dejar en blanco o el puerto utilizado, ej: 443), **Protocolo** (`http`), y la **Ruta al archivo Key** (la clave privada RSA proporcionada, ej: `picopico.key`).|
|**3. Análisis del Tráfico**|Aplicada la clave, Wireshark descifrará automáticamente las sesiones TLS. Aplicar un filtro como `http` para ver el tráfico de la capa de aplicación.|
|**4. Recuperar la Flag**|Clic derecho sobre cualquier paquete HTTP descifrado $\rightarrow$ **Follow** $\rightarrow$ **TLS Stream** o **HTTP Stream**. La _flag_ se encontrará en el **texto plano** de la comunicación (generalmente en la respuesta de un `GET` o en un encabezado).|

### Notas Adicionales

- El cifrado utilizado es **TLS (Transport Layer Security)**. La clave proporcionada es la **clave privada del servidor** que permite la desencriptación del _handshake_ y los datos de la sesión.
    
- Alternativamente, se puede utilizar la herramienta de línea de comandos **`ssldump`** con la sintaxis: `ssldump -r capture.pcap -k picopico.key -d`.
    

### Referencias

- **Herramientas:** Wireshark, ssldump
    
- **Conceptos:** Forensics de red, TLS/SSL, Desencriptación con clave privada RSA.
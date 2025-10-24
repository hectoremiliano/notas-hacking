#### Description

I've hidden a flag in this file. Can you find it? [Forensics is fun.pptm](https://mercury.picoctf.net/static/c0da20f29337e87ffb58ea987d8c596e/Forensics%20is%20fun.pptm)
**Solución**

1. **Apertura y Filtrado:** Abrir el archivo `.pcap` en **Wireshark**. Se inspecciona el tráfico, concentrándose en los protocolos de la capa de aplicación (HTTP).
    
2. **Búsqueda de Datos en Claro:** El nombre "WeakEdge" sugiere una vulnerabilidad en el cliente o en el protocolo utilizado, posiblemente el envío de información confidencial sin cifrar.
    
3. **Análisis de Solicitudes HTTP:** Se utiliza el filtro **`http`** en Wireshark. Se revisan los _streams_ y los paquetes, prestando especial atención a:
    
    - **Encabezados HTTP:** Buscar encabezados no estándar o inusuales.
        
    - **Métodos POST:** Revisar el cuerpo de las solicitudes `POST`, ya que a menudo contienen datos de formularios o credenciales.
        
    - **Contenido Sospechoso:** Buscar cualquier dato que se asemeje al formato de una _flag_ (`picoCTF{...}`).
        
4. **Recuperación de la Flag:** La _flag_ se encuentra en texto plano dentro de una de las solicitudes o respuestas HTTP descifradas, a menudo en un **campo de formulario**, **parámetro de URL** o **encabezado de respuesta** inusual.
    

**Notas Adicionales**

- Si hay tráfico TLS/HTTPS, la clave no es necesaria, ya que el desafío se enfoca en las fallas de la aplicación (_WeakEdge_) en sí, no en la desencriptación de TLS.
    
- Se puede usar **`tshark`** para analizar el archivo `.pcap` por línea de comandos, buscando cadenas específicas.
    

**Referencias**

- **Herramientas:** Wireshark, Tshark.
    
- **Conceptos:** Análisis de tráfico de red, Inspección de _payloads_ de HTTP, Vulnerabilidades de aplicaciones (ej: envío de contraseñas sin cifrar).
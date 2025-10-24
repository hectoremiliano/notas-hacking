### Solución

|**Tarea**|**Procedimiento Detallado**|
|---|---|
|**1. Desencriptación Inicial**|Seguir los mismos pasos de WebNet 0 para **cargar el `.pcap` y configurar la clave privada RSA** en Wireshark. El tráfico HTTP será visible.|
|**2. Identificar Pista/Señuelo**|Al seguir el _stream_ HTTP, se suele encontrar una _flag_ de señuelo (decoy) en un encabezado, como `Pico-Flag: picoCTF{this.is.not.your.flag.anymore}`. Esto confirma que el proceso de desencriptación es correcto, pero la _flag_ real está en otro lugar.|
|**3. Análisis de Transferencia de Archivos**|Buscar transferencias de archivos o datos grandes dentro del _stream_ descifrado. El reto a menudo involucra la descarga de una imagen (JPEG/JFIF) o de otro tipo de archivo.|
|**4. Recuperar la Flag Real**|La _flag_ real está **incrustada o camuflada** dentro de los datos del archivo transferido (el cuerpo del paquete de respuesta HTTP) o justo después de la respuesta de ese archivo. Se debe **navegar minuciosamente** por el _stream_ TLS/HTTP descifrado hasta encontrar la _flag_ con el formato `picoCTF{...}`.|

### Notas Adicionales

- La **clave RSA** es generalmente la misma que la utilizada en WebNet 0.
    
- Este reto evalúa la comprensión de que no siempre el primer indicio es la solución final, requiriendo **atención al detalle** en la inspección de datos binarios y texto plano del _stream_ descifrado.
    

### Referencias

- **Herramientas:** Wireshark (Filtros y Follow Stream), ssldump.
    
- **Conceptos:** Forensics de red avanzado, _Flags_ de señuelo (Decoy Flags), Análisis de la carga útil (Payload) de HTTP.
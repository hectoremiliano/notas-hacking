#### Description

Theres tapping coming in from the wires. What's it saying `nc jupiter.challenges.picoctf.org 48247`.
**Descripción:**  
(Asumido) Reto relacionado con audio (tap patterns) o side-channel (tapping), donde la bandera está codificada en toques/ritmo o en una grabación.

**Solución**

1. Abrir audio en `audacity`/generar espectrograma (`sox`/`ffmpeg`) y buscar patrones visuales que formen texto.
    
2. Extraer amplitud/intervalos de pulsos y mapear a bits (on/off) y convertir a ASCII.
    
3. Si son tonos DTMF, usar herramienta para decodificar y traducir a números/texto.
    

**Notas adicionales**

- Convertir audio a WAV y usar `spectrogram` ayuda a ver texto oculto.
    
- Para tapping en tiempo, detectar peaks y medir intervalos.
    

**Referencias**  
`sox`, `ffmpeg`, Audacity spectrogram.
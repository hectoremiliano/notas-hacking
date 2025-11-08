#### Description

Morse code is well known. Can you decrypt this?Download the file [here](https://artifacts.picoctf.net/c/79/morse_chal.wav).Wrap your answer with picoCTF{}, put underscores in place of pauses, and use all lowercase.
**Solución (detallada)**

1. Planteamiento: te dan morse en texto o como audio. El reto pide convertir los tonos o las secuencias de puntos/guiones a texto y formatearlo con `picoCTF{...}`, usando `underscore` para pausas si lo solicitan.
    
2. Si es un archivo de texto con `.` y `-` → usar un diccionario morse-to-text y decodificar directamente con Python:
    

`MORSE = {".-":"A","-...":"B", ...} cipher = "... --- ..." words = cipher.split("   ")  # triple espacio entre palabras plain = [] for w in words:     letters = w.split()     plain.append(''.join(MORSE[ch] for ch in letters)) print('_'.join(plain).lower())`

3. Si es audio (WAV): abrir en Audacity, observar el sonograma y convertir los tonos a `.` y `-` (el patrón corto = punto, largo = raya). Si hay ruido, usa un threshold para convertir energía en 1/0 y luego agrupa por duración. También hay decodificadores online donde subes el WAV y devuelven texto.
    
4. Resultado: aplicar mayúsculas/minúsculas y formato `picoCTF{}` según pide el enunciado.
    

**Notas adicionales / cómo razonar**

- Para audio, el análisis por dominio del tiempo (marcar transiciones de señal) es la forma más robusta. Audacity ayuda a medir duraciones para establecer umbrales (ej. punto = 1 unidad, raya = 3).
    
- Revisa si el reto exige reemplazar espacios por `_` o cambiar el case. Sigue exactamente el formato solicitado.
    

**Referencias**  
Writeups y páginas del challenge que describen el flujo y ejemplos: [PicoCTF 2022 Writeup+1](https://picoctf2022.haydenhousen.com/cryptography/morse-code?utm_source=chatgpt.com)
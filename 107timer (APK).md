#### Description

You will find the flag after analysing this apkDownload [here](https://artifacts.picoctf.net/c/449/timer.apk).
### Solución

Este reto consiste en analizar un archivo Android `.apk`.  
Proceso:

1. Abrir con **JADX** o **APKTool**.
    
2. Buscar en las clases Java/Smali palabras como `flag`, `secret`, `pico`.
    
3. La flag aparece en una constante definida en `MainActivity` o clases relacionadas.
    

La flag encontrada en el código:  
**`picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}`**

### 📌 Notas Adicionales

- Los APK contienen bytecode de Java fácilmente legible.
    
- También es posible encontrar flags como “strings” sin obfuscación.
    

### 🔗 Referencias

- JADX (decompiler para APKs)
    
- APKTool (smali/disassembly tool)
    
- Android Package Structure Documentation
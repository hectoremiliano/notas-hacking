#### Description

Python scripts are invoked kind of like programs in the Terminal... Can you run [this Python script](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py) using [this password](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/pw.txt) to get [the flag](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/flag.txt.en)?

**Solución**

- Descargar `ende.py`, `pw.txt`, `flag.txt.en`.
    
- Comprobar `pw.txt` (contiene: `aa821c16aa821c16aa821c16aa821c16`).
    
- Ejecutar: `python3 ende.py -d flag.txt.en < pw.txt`
    
- El script solicita la contraseña y muestra la flag:  
    `picoCTF{4p0110_1n_7h3_h0us3_aa821c16}`
    

**Notas adicionales**

- Flujo típico: el script espera la clave por stdin; `pw.txt` proporciona la entrada.
    
- No fue necesario romper la criptografía: el autor del reto diseñó la utilidad para usarse con esa entrada.
    

**Referencias**

- `python3`, redirección de stdin (`<`)
    
- Buenas prácticas: revisar archivos antes de ejecutar scripts descargados
#### Description

Why search for the flag when I can make a bookmarklet to print it for me?

Additional details will be available after launching your challenge instance.
## Solución (pasos esenciales)

**Entrada recibida (snippet):**

`(function() {     var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí";     var key = "picoctf";     var decryptedFlag = "";     for (var i = 0; i < encryptedFlag.length; i++) {         decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);     }     alert(decryptedFlag); })();`

**Qué hace y cómo obtener la flag:**

1. El código aplica una operación byte‑a‑byte sobre la cadena `encryptedFlag`, restando el código ASCII del carácter de la `key` correspondiente (repetida cíclicamente), luego normaliza el resultado con `+256 % 256`.
    
2. Esa operación es una variante simple de cifrado por _XOR/cesar/vigenère sobre bytes_ (aquí suma/resta modular sobre bytes en rango 0–255).
    
3. Para obtener la flag puedes ejecutar exactamente ese snippet en la consola del navegador (DevTools → Console) o replicarlo en Python.
    
    - En la consola del navegador: pegar el snippet y aceptar la alerta mostrará la bandera.
        
    - En Python (ejemplo):
        
    
    `enc = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí" key = "picoctf" dec = "".join(chr((ord(enc[i]) - ord(key[i % len(key)]) + 256) % 256) for i in range(len(enc))) print(dec)`
    
4. Resultado (flag):  
    `picoCTF{p@g3_turn3r_0c0d211f}`
    

---

## Notas adicionales

- La rutina usa aritmética modular sobre bytes (`(a - b + 256) % 256`) para evitar valores negativos y mantener resultados en el rango 0–255.
    
- Aunque el snippet alerta la flag en el navegador, es preferible decodificar localmente (por ejemplo en Python) para registrar el resultado sin ventanas emergentes.
    
- Este esquema no es criptográficamente seguro: es simplemente una ofuscación reversible si se conoce la clave.
    
- No publiques flags en foros durante la competición; preservar integridad del CTF y normas de divulgación.
    

---

## Referencias

- `String.fromCharCode`, `charCodeAt` — MDN Web Docs.
    
- Conceptos: Vigenère cipher (operación similar pero en bytes), aritmética modular.
    
- Uso práctico: abrir DevTools (F12) → pestaña _Console_ para ejecutar JavaScript snippets.
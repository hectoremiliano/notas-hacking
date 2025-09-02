#### Description

What does this `bDNhcm5fdGgzX3IwcDM1` mean? I think it has something to do with bases.

# Solución

La cadena `bDNhcm5fdGgzX3IwcDM1` es **Base64**. Decodificándola obtenemos:

`l3arn_th3_r0p35`

Interpretando la _leet-speak_ (3 → e, 0 → o, 5 → s) se obtiene la frase legible:

`learn_the_ropes`

Por lo tanto el flag es:

picoCTF{l3arn_th3_r0p35}

---

# Código en Python

`import base64  s = "bDNhcm5fdGgzX3IwcDM1" decoded = base64.b64decode(s).decode('utf-8') print("Decoded:", decoded)  # convertir leet a texto legible (simple sustitución) leet_map = str.maketrans({"3":"e", "0":"o", "5":"s", "1":"i", "4":"a", "7":"t"}) plain = decoded.translate(leet_map) print("Plain:", plain)`

Salida:

`Decoded: l3arn_th3_r0p35 Plain: learn_the_ropes`

---

# Notas adicionales

- La presencia de caracteres base64 (`A–Z`, `a–z`, `0–9`, `+`, `/`) y la longitud típica indican codificación Base64.
    
- Muchos retos ocultan texto legible usando Base64 y luego _leet-speak_ u otras sustituciones.
    
- En Python `base64.b64decode()` decodifica Base64; `str.translate()` con un mapa de sustitución sirve para convertir leet a texto legible.
    

# Referencias

- Documentación Python: módulo `base64`.
    
- Herramientas online comunes: CyberChef (para detectar/decodificar Base64 y aplicar sustituciones).
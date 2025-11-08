#### Description

We received an encrypted message. The modulus is built from primes large enough that factoring them isn’t an option, at least not today. See if you can make sense of the numbers and reveal the flag.Download the [message](https://challenge-files.picoctf.net/c_amiable_citadel/3dcd347a6e654c8e7e4c10bd3ad888f73c14cb5375535053baef6ed5cbe2baa8/message.txt).
**Solución (detallada)**

1. Planteamiento: te dan `n, e, c`. Observa `e`. Si `e` es inusualmente pequeño (por ejemplo 3, 5, 7, 20 en algunos retos), y el mensaje `m` (interpretado como entero) satisface `m^e < n`, entonces la encriptación modular no “envuelve” y `c = m^e` exactamente. En ese caso, `m = round_root_e(c)` (la raíz entera e-ésima).
    
2. Pasos prácticos:
    
    - Comprueba tamaños: si `c < n` y `e` pequeño, intenta calcular la raíz entera `m = ⌊c^(1/e)⌋` verificando `m**e == c`.
        
    - Si `m**e != c`, puede que el mensaje esté empaquetado (padding) o que exista una envoltura modular; entonces se necesitan otros métodos (factorizar `n`, ataques de compartición, Wiener, etc.).
        
3. Código ejemplo (Python con gmpy2 o con integer_nth_root):
    

`import gmpy2 c = <valor_c> e = <valor_e> m, exact = gmpy2.iroot(c, e) if exact:     print("Mensaje recuperado:", long_to_bytes(int(m))) else:     print("No es raíz exacta; probar otros ataques")`

4. Resultado: si la raíz fue exacta y la conversión a bytes es válida, la bandera aparece como texto ASCII.
    

**Notas adicionales / cómo razonar**

- Este ataque no requiere factorizar `n`. Se aprovecha únicamente de `e` muy pequeño y mensajes pequeños sin padding.
    
- Si el challenge utiliza padding (p. ej. PKCS#1), `m` será más grande y este ataque fallará.
    
- Herramientas útiles: `gmpy2`, `sympy.integer_nthroot` o funciones propias para raíz entera. También `rsatool`/`RsaCtfTool` prueban muchas de estas y otras vulnerabilidades automáticamente.
    

**Referencias**  
Explicaciones y ejemplos del mismo problema en writeups y blogs: [Medium+2Medium+2](https://medium.com/%40himanshu15608/picoctf-by-cmu-africa-42137ad0ef09?utm_source=chatgpt.com)
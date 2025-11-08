#### Description

This service provides you an encrypted flag. Can you decrypt it with just N & e?

Additional details will be available after launching your challenge instance.
**Solución (detallada)**

1. Planteamiento rápido: te dan `N` (módulo), `e` (exponente público) y un cifrado `c`. El objetivo es recuperar `m` (el mensaje/flag). RSA asume que `N = p * q` con `p` y `q` primos impares — si **N es par**, uno de los factores es 2 y la seguridad se rompe inmediatamente.
    
2. Comprobación inicial: mira si `N % 2 == 0`. Si es verdadero, `p = 2` y `q = N // 2`.
    
3. Calcula `phi = (p-1)*(q-1)` y el inverso multiplicativo `d = e^{-1} mod phi`. Con `d` calcula `m = c^d mod N`. Finalmente formatea `m` a bytes/ASCII para obtener la bandera.
    
4. Código mínimo (Python, con `pow` y `inverse` de Crypto.Util):
    

`from Crypto.Util.number import inverse, long_to_bytes N = <valor_N> e = <valor_e> c = <valor_c> if N % 2 == 0:     p = 2     q = N//2     phi = (p-1)*(q-1)     d = inverse(e, phi)     m = pow(c, d, N)     print(long_to_bytes(m)) else:     print("N no es par; prueba otros ataques")`

5. Resultado: la bandera será el texto resultante dentro del formato esperado (p. ej. `picoCTF{...}`).
    

**Notas adicionales / cómo razonar**

- Siempre realiza comprobaciones rápidas antes de atacar con factorizaciones costosas: divisibilidad por 2, 3, 5, GCD con otros módulos, si `e` es pequeño, etc. Estas checadas rápidas detectan malas configuraciones que aparecen en retos.
    
- El caso “N par” es una mala generación de claves (uno de los primos fue 2). Es triviale identificarlo y explotarlo.
    
- Si `N` no es par, otros vectores comunes: primos compartidos entre múltiples `N` (usar `gcd(N1,N2)`), `e` pequeño con `m^e < N` (ver Crack the Power) o `e` vulnerable a Wiener (d pequeño).
    

**Referencias**  
Writeups y explicaciones del reto y variantes similares: [Cbarkr Blog+3Medium+3System Weakness](https://medium.com/%40ibnuilham/even-rsa-can-be-broken-348c8455db50?utm_source=chatgpt.com)
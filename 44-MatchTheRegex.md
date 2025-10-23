#### Description

How about trying to match a regular expression

Additional details will be available after launching your challenge instance.
### **Solución** 

**Idea clave:** invertir la regex: entender qué acepta (o generar automáticamente una cadena válida). Herramientas útiles: leer y simplificar la regex a mano, usar generadores como `exrex`/`rstr` o utilidades online (regex101) para probar, o escribir un pequeño script que genere candidatos (backtracking controlado o búsqueda por fuerza/bruta guiada).

1. **Obtener la regex**
    
    - Copia exactamente la expresión regular que te dan en el reto. Si viene dentro de JavaScript/HTML, mira bien las barras `/.../` y los flags (`i`, `m`, `s`).
        
2. **Inspección manual rápida**
    
    - Identifica anclas `^` / `$` (requiere coincidencia completa).
        
    - Observa clases `[...]`, cuantificadores `{m,n}`, grupos `(...)` y alternancias `|`.
        
    - Busca secuencias literales (por ejemplo `picoCTF`, `flag`, `{`, `}`) o patrones que indiquen el formato de la bandera (p. ej. `picoCTF\{[A-Za-z0-9_]+\}`).
        
3. **Probar con regex tester (rápido)**
    
    - Pega la regex en un tester (ej. regex101) y prueba cadenas manuales hasta ver coincidencia. Esto ayuda a entender restricciones (longitud, caracteres permitidos).
        
4. **Generación automática (si la regex es complicada)**
    
    - Usa `exrex` (Python) o `rstr` para generar una cadena que cumpla la regex:
        
        `# instalar exrex (opcional) pip install exrex # generar una muestra exrex 'TU_REGEX_AQUI' | head -n 5`
        
    - Con `rstr` (random):
        
        `import rstr print(rstr.xeger(r'TU_REGEX_AQUI'))`
        
    - Si la regex usa anclas y cuantificadores grandes, ajusta parámetros (ej. limitar repeticiones).
        
5. **Si la regex tiene partes con restrictions numéricas/combinatorias**
    
    - Desglosa en segmentos y genera parte por parte; arma la cadena final concatenando las partes válidas.
        
    - Si hay alternancias con muchas opciones, prioriza alternativas literales que contengan patrones visibles (picoCTF, flag, etc.).
        
6. **Brute-force guiado (cuando la regex es corta o muy restrictiva)**
    
    - Si la longitud es pequeña (≤10) y el alfabeto restringido, haz búsqueda por BFS/DFS: genera cadenas ordenadas lexicográficamente y testea `re.fullmatch(regex, candidate)`. Ejemplo en Python:
        
        `import re, itertools, string regex = re.compile(r'TU_REGEX_AQUI') alphabet = 'abcdefghijklmnopqrstuvwxyz0123456789{}_' maxlen = 8 for L in range(1, maxlen+1):     for s in itertools.product(alphabet, repeat=L):         cand = ''.join(s)         if regex.fullmatch(cand):             print('Found:', cand)             raise SystemExit`
        
7. **Enviar la cadena hallada**
    
    - Usa el cliente que te provee el reto (formulario web, socket, etc.) y manda la cadena. Si la regex estaba ligada a la bandera, la respuesta del servicio te mostrará la flag (p. ej. `picoCTF{...}`).
        

---

### **Notas adicionales**

- Si la regex es excesivamente compleja u ofuscada (lookaheads/lookbehinds/condicionales), probar generación automática con `exrex` o `rstr` suele ahorrar mucho tiempo.
    
- Ten cuidado con cuantificadores “greedy” y rangos enormes (`{1,10000}`): al generar, limita repeticiones razonables.
    
- Si la regex contiene partes dinámicas (valores calculados en JS), revisa el código cliente para ver cómo se construye la regex antes de invertirla.
    
- Para regexes con anclas y escapes (`\xhh`), convierte escapes hex a caracteres reales antes de generar.
    
- Herramientas locales te permiten iterar sin exponer la cadena a la web pública; trabaja localmente hasta tener la candidata correcta.
    

---

### **Referencias**

- `exrex` — generar cadenas desde una regex (Python).
    
- `rstr.xeger` (rstr) — generar strings que cumplan una regex.
    
- Regex101 — tester interactivo para probar regex y ver explicación de cada parte.
    
- Documentación `re` de Python — `fullmatch`, `search`, flags y escapes.
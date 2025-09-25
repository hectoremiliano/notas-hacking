#### Description

Can you find the flag on this website.

Additional details will be available after launching your challenge instance.
**Solución**

- Entorno: Kali Linux con Burp Suite y FoxyProxy (útil para interceptar formularios), sqlite3 si quieres probar consultas localmente.
    
- Visitar la página; identificar el formulario vulnerable (login o búsqueda).
    
- Intentar bypass básico de autenticación con la clásica carga:
    

' OR 1=1-- -

- Si el site permite consultas más complejas (por ejemplo buscar ciudades), usar `UNION SELECT` para extraer columnas o tablas del esquema SQLite. Ejemplos de payloads:
    

# union usando una entrada tipo: Algiers" UNION SELECT sql,1,1 FROM sqlite_master;--

# o para extraer flag de una tabla creada: Algiers" UNION SELECT flag, id, 1 FROM more_tables;--

- `sqlite_master` es la tabla del sistema en SQLite que lista tablas y columnas; consultarla puede revelar el nombre de la tabla que contiene la flag.
    
- Una vez conocida la tabla y columna, usar un `UNION SELECT` que inyecte la columna `flag` para que aparezca en el resultado mostrado por la página.
    

**Ejemplo con curl**

# payload como parámetro de búsqueda (URL-encode según sea necesario)

curl -s "http://TARGET/?q=Algiers%22%20UNION%20SELECT%20flag,id,1%20FROM%20more_tables;--"

**Notas adicionales**

- Prueba primero con `OR 1=1` para confirmar vulnerabilidad; después enumera tablas con `sqlite_master`.
    
- Burp Repeater y Proxy son muy útiles para iterar payloads sin reescribir cabeceras/manualmente.
    
- Si la base de datos no es SQLite, los `UNION SELECT` pueden necesitar ajustar número y tipo de columnas.
    

**Referencias**

- https://siunam321.github.io/ctf/picoCTF-2023/Web-Exploitation/More-SQLi/
    
- https://medium.com/@Gumn4m1/more-sqli-5fb0589a147b
    
- https://www.youtube.com/watch?v=W1EjP6OFpUQ
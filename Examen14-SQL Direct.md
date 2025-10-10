#### Description

Connect to this PostgreSQL server and find the flag!

Additional details will be available after launching your challenge instance.
## Solución

1. **Instalar cliente de PostgreSQL** (si no está presente):
    

`sudo apt update sudo apt install postgresql-client`

2. **Conectar al servidor PostgreSQL remoto** usando `psql` especificando host, puerto, usuario y base de datos:
    

`psql -h saturn.picoctf.net -p 58962 -U postgres pico`

Se ingresa la contraseña cuando `psql` la solicita.

3. **Listar tablas** del esquema actual:
    

`\dt`

Salida esperada (ejemplo):

`List of relations  Schema | Name  | Type  |  Owner --------+-------+-------+----------  public | flags | table | postgres`

4. **Consultar el contenido de la tabla**:
    

`select * from flags;`

Resultado obtenido:

 `id | firstname | lastname  |                address ----+-----------+-----------+----------------------------------------   1 | Luke      | Skywalker | picoCTF{L3arN_S0m3_5qL_t0d4Y_21c94904}   2 | Leia      | Organa    | Alderaan   3 | Han       | Solo      | Corellia`

5. **Flag encontrada** (campo `address` del registro id=1):  
    **`picoCTF{L3arN_S0m3_5qL_t0d4Y_21c94904}`**
    

---

## Notas adicionales

- `psql` es la herramienta cliente estándar para interactuar con servidores PostgreSQL; permite ejecutar consultas SQL, metacomandos (`\dt`, `\l`, `\d table`, etc.) y scripts.
    
- En entornos remotos siempre usar opciones `-h` (host) y `-p` (puerto) cuando el servidor no está en localhost.
    
- El prompt de `psql` (`pico=#`) indica que la conexión se realizó con éxito y se tiene acceso a la base de datos `pico` como usuario `postgres`.
    
- Verifica siempre el **contexto de permisos**: en este caso el usuario tenía privilegios para listar y seleccionar la tabla `flags`. En sistemas reales esto puede ser una mala práctica si usuarios no autorizados pueden leer datos sensibles.
    
- Buenas prácticas de seguridad:
    
    - No compartir contraseñas ni credenciales en documentación pública.
        
    - Usar conexiones seguras (TLS) cuando sea posible.
        
    - Limitar privilegios de usuarios a lo estrictamente necesario (principio de mínimo privilegio).
        
    - Auditar y sanitizar accesos a datos sensibles.
        
- `psql` muestra la versión del cliente y del servidor en la cabecera; en este caso el cliente era 16.10 y el servidor 15.2 — útil para compatibilidad de funcionalidades.
    

---

## Referencias

- Documentación oficial de `psql` (PostgreSQL): [https://www.postgresql.org/docs/current/app-psql.html](https://www.postgresql.org/docs/current/app-psql.html)
    
- Guía rápida de comandos `psql` (metacomandos como `\dt`, `\d`, `\l`): [https://www.postgresql.org/docs/current/app-psql.html#APP-PSQL-META-COMMANDS](https://www.postgresql.org/docs/current/app-psql.html#APP-PSQL-META-COMMANDS)
    
- Tutorial SQL básico (SELECT, JOIN, CREATE TABLE): [https://www.postgresql.org/docs/current/tutorial-sql.html](https://www.postgresql.org/docs/current/tutorial-sql.html)
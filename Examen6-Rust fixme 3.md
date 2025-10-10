#### Description

Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/dcdaf491b35c1d0f5075e9583edbbb7aaea1dffb6ad32bc000e4d87b5200ff7b/fixme3.tar.gz).
**Solución**

- Descargar y extraer `fixme3.tar.gz`.
    
- `cargo build` produjo error: llamada a función `unsafe` (`std::slice::from_raw_parts`) fuera de bloque `unsafe`.
    
- Editar `src/main.rs`, envolver la llamada en un bloque `unsafe { ... }`.
    
- Ejecutar `cargo build` y `cargo run`; programa imprime la bandera:  
    `picoCTF{n0w_y0uv3_f1x3d_1h3m_411}`
    

**Notas adicionales**

- Enseña la filosofía de Rust: llamadas no seguras deben marcarse explícitamente con `unsafe`.
    
- Atención: operación con punteros y slices requiere cuidado por comportamiento indefinido.
    

**Referencias**

- Rust Nomicon (unsafe Rust)
    
- Documentación `std::slice::from_raw_parts`
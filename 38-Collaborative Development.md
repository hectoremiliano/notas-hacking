#### Description

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/177/challenge.zip)
## Solución

1. Descargué y descomprimí el archivo `challenge.zip`.
    
2. Dentro del contenido encontré un directorio `.git/objects` que contenía blobs ocultos con información.
    
3. Usando los comandos de Git (`git cat-file -p <hash>`) revisé los objetos y descubrí que la flag estaba dividida en varias partes dentro de diferentes blobs.
    
4. Reconstruí las partes hasta formar la flag completa:
    

`picoCTF{t3@mw0rk_w0rk_7ae8dd33}`

---

## Notas adicionales

- Este reto pone a prueba la exploración de repositorios Git, ya que muchas veces la información sensible no está en los archivos visibles sino en los objetos del repositorio.
    
- Fue clave revisar manualmente los blobs en `.git/objects` y unir el contenido.
    
- El tiempo de resolución fue de aproximadamente **1 minuto y 5 segundos**, lo cual indica que una vez identificado que era un reto de Git, la extracción de la flag fue directa.
Referencias
https://www.youtube.com/watch?v=_CH5IQewkzw&ab_channel=AnonymousWorld
#### Description

Want to play a game? As you use more of the shell, you might be interested in how they work! Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses.Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, and forensics. Practicing the fundamentals manually might help you in the future when you have to write your own tools!You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_atlas/5/challenge.zip)

`ssh -p 57108 ctf-player@atlas.picoctf.net`Using the password `1ad5be0d`. Accept the fingerprint with `yes`, and `ls` once connected to begin. Remember, in a shell, passwords are hidden!

## Solución

1. Localicé el script del reto:
    
    `ls # -> drop-in/guessing_game.sh`
    
2. Ejecución del juego:
    
    `bash drop-in/guessing_game.sh`
    
    El juego imprime un mensaje tipo:
    
    `Welcome to the Binary Search Game! I'm thinking of a number between 1 and 1000.`
    
3. Procedí a introducir números (intentos) hasta adivinar el número objetivo. Cuando aciertas, el script lee la flag de `/challenge/metadata.json` y la imprime:
    
    `Congratulations! You guessed the correct number: <número> Here's your flag: <flag>`
    
4. Resultado: la flag se mostró por la salida estándar tras adivinar el número correcto (el script extrae la flag con `jq` desde `/challenge/metadata.json`).
    

---

## Notas adicionales

- **Mecánica:** el juego genera un número aleatorio entre 1 y 1000 y espera entradas por stdin; no acepta salir por señales (hay traps para INT, SIGQUIT, TSTP).
    
- **Única forma práctica (si no tienes el metadata):** introducir números hasta adivinar — en el peor caso 1000 intentos.
    
- **Automatización:** para no teclear a mano, se puede automatizar con un script que pruebe 1..1000 en secuencia, por ejemplo:
    
    `for i in $(seq 1 1000); do   echo "$i" done | bash drop-in/guessing_game.sh`
    
    (Esto envía las conjeturas al programa hasta que encuentre el correcto; puede cerrarlo cuando imprima la línea con `"Here's your flag:"`.)
    
- **Método alternativo y recomendado:** buscar el archivo `metadata.json` dentro del entregable (ZIP) o en el entorno del reto. Si tienes acceso a `/challenge/metadata.json`, extraer la flag es inmediato:
    
    `jq -r '.flag' /challenge/metadata.json # o grep -o 'picoCTF{[^}]*}' /challenge/metadata.json`
    
- **Seguridad:** no intentes matar el proceso con señales: el script está diseñado para evitarlo. Automatizar las pruebas (como el `for` anterior) es la forma más rápida si no puedes leer `metadata.json`.
    
- **Evidencia:** el script contiene la línea que recupera la flag:
    
    `flag=$(cat /challenge/metadata.json | jq -r '.flag') echo "Here's your flag: $flag"`
    
    lo que confirma que la flag proviene de ese JSON y **no** está embebida directamente en el script.
Referencias
https://www.youtube.com/watch?v=GIY_rURHsyA&pp=ygUVYmluYXJ5IHNlYXJjaCBwaWNvY3Rm
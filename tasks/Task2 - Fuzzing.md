# Task 2 Fuzzing de directorios web

## Información general 
- **Herramientas utilizadas:** dirb, gobuster 

---

## Investigación Inicial

Se 

---

## 1) ¿Cuantos ficheros tiene el servidor con dirb?
Una vez que se obtiene el fichero, se procede a usar dirb http://10.129.183.158 /home/kali/Downloads/wordlist.txt

En este caso DIRB encontró 17 rutas accesibles en el servidor mediante un proceso de fuerza bruta de directorios y archivos usando una wordlist(fichero descargado previamente). Estas rutas representan posibles directorios o ficheros expuestos a través de HTTP, pero no reflejan el sistema de archivos completo del servidor, sino únicamente lo que está públicamente accesible desde la web.

---

## 2) ¿Cuantos ficheros tiene el servidor con Gobuster?
En cambio, con Gobuster, se utilizó el comando 

gobuster dir -u http[:]//10.129.183.158 -w /home/kali/Downloads/wordlist.txt" 

y se enumeraron directorios y archivos en el servidor web y la herramienta identificó múltiples rutas, incluyendo archivos con código fuente y copias de seguridad. Algunas rutas devolvieron código 403 (Forbidden), indicando que existen en el servidor pero no son accesibles directamente. Otras rutas, como security.zip y config.php.bak, devolvieron código 200, lo que indica que son accesibles públicamente y podrían contener información sensible.
Algunos ejemplos de estas rutas son: 
- /config.php.bak         (Status: 200) [Size: 2160]
- /configuration.inc.php~ (Status: 403) [Size: 345]
- /configuration~         (Status: 403) [Size: 345]

---

## 3) ¿Cuál es el código correcto?
El 

---

## 4) ¿Cuál es el código de error?
El código de error es 

---

## 5) ¿Cuál es la flag para que no muestre los ficheros erróneos en dirb?
Al 

---

## 6) ¿Cuál es el tamaño en bytes de fichero de mayor tamaño?
El 

---

## 7) ¿Cómo se llama el fichero que tiene un tamaño inferior al mayor?
La 

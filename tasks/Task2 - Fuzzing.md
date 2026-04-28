# Task 2 Fuzzing de directorios web

## Información general 
- **Herramientas utilizadas:** dirb, gobuster 

---

## Investigación Inicial

Se realizó un reconocimiento inicial sobre el servidor web objetivo para identificar posibles puntos de entrada y recursos expuestos. Para ello, se emplearon herramientas de fuzzing de directorios como dirb y gobuster, las cuales permiten descubrir rutas ocultas o no enlazadas directamente desde la página principal. 

---

## 1) ¿Cuántos ficheros tiene el servidor con dirb?
Una vez descargada la wordlist, se procede a usar dirb:

`dirb http[:]//10.130.178.202 /home/kali/Downloads/wordlist.txt`

En este caso DIRB encontró 17 rutas accesibles en el servidor mediante un proceso de fuerza bruta de directorios y archivos usando una wordlist (fichero descargado previamente). Estas rutas representan posibles directorios o ficheros expuestos a través de HTTP, pero no reflejan el sistema de archivos completo del servidor, sino únicamente lo que está públicamente accesible desde la web.

```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.130.178.202 /home/kali/Downloads/wordlist.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Apr 28 13:03:19 2026
URL_BASE: http://10.130.178.202/
WORDLIST_FILES: /home/kali/Downloads/wordlist.txt

-----------------

GENERATED WORDS: 1018                                                          

---- Scanning URL: http://10.130.178.202/ ----
+ http://10.130.178.202/cfg~ (CODE:403|SIZE:345)                                                      
+ http://10.130.178.202/cfg.php~ (CODE:403|SIZE:345)                                                  
+ http://10.130.178.202/conf~ (CODE:403|SIZE:345)                                                     
+ http://10.130.178.202/config~ (CODE:403|SIZE:345)                                                   
+ http://10.130.178.202/configgconfig.php~ (CODE:403|SIZE:345)                                        
+ http://10.130.178.202/config.inc.php~ (CODE:403|SIZE:345)                                           
+ http://10.130.178.202/config.php.bak (CODE:200|SIZE:2160)                                           
+ http://10.130.178.202/configuration~ (CODE:403|SIZE:345)                                            
+ http://10.130.178.202/configuration.inc.php~ (CODE:403|SIZE:345)                                    
+ http://10.130.178.202/conf.inc.php~ (CODE:403|SIZE:345)                                             
+ http://10.130.178.202/database.php~ (CODE:403|SIZE:345)                                             
+ http://10.130.178.202/Db.php~ (CODE:403|SIZE:345)                                                   
+ http://10.130.178.202/functions.php~ (CODE:403|SIZE:345)                                            
+ http://10.130.178.202/index.php~ (CODE:403|SIZE:345)                                                
+ http://10.130.178.202/.php~ (CODE:403|SIZE:345)                                                     
+ http://10.130.178.202/security.zip (CODE:200|SIZE:2109)                                             
+ http://10.130.178.202/sliding_contact.php~ (CODE:403|SIZE:345)                                      
                                                                                                      
-----------------
END_TIME: Tue Apr 28 13:04:13 2026
DOWNLOADED: 1018 - FOUND: 17

```

---

## 2) ¿Cuantos ficheros tiene el servidor con Gobuster?
En cambio, con Gobuster, se utilizó el comando 

`gobuster dir -u http[:]//10.130.178.202 -w /home/kali/Downloads/wordlist.txt` 

Mediante este se enumeraron directorios y archivos en el servidor web y la herramienta identificó múltiples rutas, incluyendo archivos con código fuente y copias de seguridad. Algunas rutas devolvieron código 403 (Forbidden), indicando que existen en el servidor pero no son accesibles directamente. Otras rutas, como security.zip y config.php.bak, devolvieron código 200, lo que indica que son accesibles públicamente y podrían contener información sensible.

```bash
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u http://10.130.178.202 -w /home/kali/Downloads/wordlist.txt 
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.130.178.202
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /home/kali/Downloads/wordlist.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/cfg~                 (Status: 403) [Size: 345]
/cfg.php~             (Status: 403) [Size: 345]
/configgconfig.php~   (Status: 403) [Size: 345]
/config~              (Status: 403) [Size: 345]
/conf~                (Status: 403) [Size: 345]
/config.php.bak       (Status: 200) [Size: 2160]
/config.inc.php~      (Status: 403) [Size: 345]
/configuration.inc.php~ (Status: 403) [Size: 345]
/configuration~       (Status: 403) [Size: 345]
/conf.inc.php~        (Status: 403) [Size: 345]
/database.php~        (Status: 403) [Size: 345]
/Db.php~              (Status: 403) [Size: 345]
/functions.php~       (Status: 403) [Size: 345]
/index.php~           (Status: 403) [Size: 345]
/.php~                (Status: 403) [Size: 345]
/security.zip         (Status: 200) [Size: 2109]
/sliding_contact.php~ (Status: 403) [Size: 345]
/cfg~                 (Status: 403) [Size: 345]
/cfg.php~             (Status: 403) [Size: 345]
/conf~                (Status: 403) [Size: 345]
/config~              (Status: 403) [Size: 345]
/config.inc.php~      (Status: 403) [Size: 345]
/configgconfig.php~   (Status: 403) [Size: 345]
/configuration.inc.php~ (Status: 403) [Size: 345]
/configuration~       (Status: 403) [Size: 345]
/conf.inc.php~        (Status: 403) [Size: 345]
/database.php~        (Status: 403) [Size: 345]
/Db.php~              (Status: 403) [Size: 345]
/functions.php~       (Status: 403) [Size: 345]
/index.php~           (Status: 403) [Size: 345]
/.php~                (Status: 403) [Size: 345]
/sliding_contact.php~ (Status: 403) [Size: 345]
Progress: 2029 / 2029 (100.00%)
===============================================================
Finished
===============================================================         
```

---

## 3) ¿Cuál es el código correcto?
El código de estado HTTP correcto (éxito) que aparece en los resultados es el 200, el cual se muestra únicamente para los archivos config.php.bak y security.zip. Esto indica que ambos recursos existen en el servidor y son accesibles.

```bash
config.php.bak       (Status: 200) [Size: 2160]
security.zip         (Status: 200) [Size: 2109]
```

---

## 4) ¿Cuál es el código de error?
El código de error predominante en el escaneo es el 403  que significa Forbidden, que aparece en casi todos los archivos listados (por ejemplo, cfg~, config.inc.php~, database.php~, etc...). Este código indica que, aunque el servidor reconoce la existencia de esos archivos, el acceso a ellos está denegado explícitamente, probablemente por configuraciones de permisos o reglas de seguridad.

---

## 5) ¿Cuál es la flag para que no muestre los ficheros erróneos en dirb?
El comando utilizado para que no se muestren estos ficheros es el **`-N 403`**. Con este comando, dirb realizará el escaneo de manera normal, pero no mostrará en la lista final ningún directorio o archivo que devuelva un error 403, limpiando así la salida y permitiendo identificar más fácilmente los recursos a los que realmente se tiene acceso.

---

## 6) ¿Cuál es el tamaño en bytes de fichero de mayor tamaño?
El fichero de mayor tamaño es:

`config.php.bak        (Status: 200)   [Size: 2160]`

Esto se debe a que al ser un recurso real y accesible que contiene datos legítimos del servidor, su tamaño naturalmente es mayor porque almacena información útil y potencialmente sensible. Por lo tanto, el mayor tamaño de este archivo refleja que el servidor está sirviendo contenido real en lugar de una página de error estándar, cuyo peso es de 345 bytes, lo que lo convierte en un objetivo de mayor interés para un atacante o auditor de seguridad.

---

## 7) ¿Cómo se llama el fichero que tiene un tamaño inferior al mayor?
El segundo fichero de mayor tamaño es: 

`security.zip         (Status: 200) [Size: 2109]`

Y al igual que el fichero previamente descrito, tiene un tamaño mayor al ser un archivo quie contiene datos dentro del servidor.

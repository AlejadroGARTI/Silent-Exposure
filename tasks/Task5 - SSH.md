# Task 5 Acceso al sistema mediante SSH

## Información general 
- **Herramientas utilizadas:**  

---

## Investigación Inicial

Se 

---

## 1) ¿Cuál es el admistrador del sistema?
El administrador es dnedry, mismo usuario que encontramos inspeccionando el .bak, en el campo: 

/** Server username */
define('USER', 'dnedry');

---

## 2) ¿Cuál es su contraseña?
Se identificó que la clave pública GPG contenía información sensible en su campo UID, incluyendo una posible contraseña (L3Dodgson5). Dado que los metadatos de las claves públicas son accesibles sin autenticación, esto representa una fuga de credenciales. Esta mala práctica de gestión de secretos permitió el acceso por SSH sin necesidad de realizar ataques criptográficos, evidenciando un fallo humano.

---

## 3) ¿Ha que directorio se accede por defecto?
Se accede al directorio /home/dnedry, esto se puede saber utilizando el comando: dnedry@lab-nublar-os:~$ pwd

---

## 4) ¿Cuántos usuarios existen en el sistema?
Contando los usuarios medainte el comando: wc -l /etc/passwd, tenemos un total de 28 usuarios

---

## 5) ¿Cuál es el nombre completo del usuario?
El nombre completo del usuario es: Dennis Nedry, ya que usando cat /etc/passwd, podemos observar la siguiente línea con el nombre completo:

dnedry:x:1000:1000:Dennis Nedry,,,:/home/dnedry:/bin/bash

---

## 6) ¿Quén es el encargado de monitorear todos los sistemas?
El 

---

## 7) ¿Cuál es el hostname?
El hostnanme es: lab-nublar-os, y se obtiene al escribir hostname. 

---

## 8) Según la información del sistema, ¿en qué tipo de distribución está basado InGenOS?
Esta basado en securityos, ya que mediante el comando cat /etc/os-release, obtenemos:

- NAME="InGenOS (Jurassic Park Control System)"
- VERSION="1.0.4 (Nublar Edition)"
- ID=ingenos
- ID_LIKE=securityos
- PRETTY_NAME="InGenOS 1.0.4 LTS"
- VERSION_ID="1.0.4"
- HOME_URL="http[:]//www[.]jurassicpark.com/"
- SUPPORT_URL="http[:]//help.ingen.com/"
- BUG_REPORT_URL="http[:]//bugs.ingen.com/"

---

## 9) ¿A qué grupos pertenece el usuario?
El usuario pertenece a los grupos: uid=1000(dnedry) gid=1000(dnedry) groups=1000(dnedry),4(adm),24(cdrom),30(dip),46(plugdev),114(lpadmin),115(sambashare)

Saber esto es crucial para definir las capacidades reales que tiene el usuario dentro del sistema.







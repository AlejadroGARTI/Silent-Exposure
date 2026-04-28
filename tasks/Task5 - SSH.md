# Task 5 Acceso al sistema mediante SSH

## Información general 
- **Herramientas utilizadas:**  Comandos básicos en Kali Linux

---

## Investigación Inicial

Se realizó un análisis de los archivo .bak y .asc, proporcionado en la fase anterior, donde se identificó al administrador del sistema mediante la constante define('USER', 'dnedry'). Posteriormente, se inspeccionó la clave pública GPG del usuario, detectando que el campo UID contenía una posible contraseña en texto claro (L3Dodgson5). Esta mala práctica permitió el acceso por SSH sin necesidad de ataques criptográficos.

Con las credenciales obtenidas, se estableció una conexión SSH al servidor remoto, autenticándose como dnedry. Una vez dentro, se procedió a la recolección de información del sistema mediante comandos básicos de enumeración (pwd, hostname, cat /etc/passwd, cat /etc/os-release, id, etc.). 

---

## 1) ¿Cuál es el admistrador del sistema?
El administrador es dnedry, mismo usuario que encontramos inspeccionando el .bak, en el campo: 

```bash
/** Server username */
define('USER', 'dnedry');
```

---

## 2) ¿Cuál es su contraseña?
Se identificó que la clave pública GPG contenía información sensible en su campo UID, incluyendo una posible contraseña (L3Dodgson5). Dado que los metadatos de las claves públicas son accesibles sin autenticación, esto representa una fuga de credenciales. Esta mala práctica de gestión de secretos permitió el acceso por SSH sin necesidad de realizar ataques criptográficos, evidenciando un fallo humano.

---

## 3) ¿Ha que directorio se accede por defecto?
Se accede al directorio /home/dnedry, esto se puede saber utilizando el comando: dnedry@lab-nublar-os:~$ `pwd`

---

## 4) ¿Cuántos usuarios existen en el sistema?
Contando los usuarios medainte el comando: `wc -l /etc/passwd`, tenemos un total de 28 usuarios

Saber la cantidad de usuarios en un sistema es importante porque permite evaluar la superficie de ataque, ya que cada usuario adicional (especialmente si tiene privilegios, claves débiles o está inactivo) es un posible punto de entrada para un atacante, además un número anormalmente alto o bajo puede indicar huellas de un intruso que creó cuentas ocultas, o por el contrario, falta de control de accesos.

---

## 5) ¿Cuál es el nombre completo del usuario?
El nombre completo del usuario es: Dennis Nedry, ya que usando `cat /etc/passwd`, podemos observar la siguiente línea con el nombre completo:

dnedry:x:1000:1000:Dennis Nedry,,,:/home/dnedry:/bin/bash

```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
((dnedry:x:1000:1000:Dennis Nedry,,,:/home/dnedry:/bin/bash))
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin

```
---

## 6) ¿Quén es el encargado de monitorear todos los sistemas?
El encargado se obtiene en la informacón obtenida a la hora de iniciar la sesión remota y autenticarse, vemos al  encargado de monitorear el sistema.

```bash
The server may need to be upgraded. See https://openssh.com/pq.html
dnedry@10.130.178.202's password: 
 
  ___          ____
 |_ _| _ __   / ___|  ___  _ __
  | | | '_ \ | |  _  / _ \| '_ \
  | | | | | || |_| ||  __/| | | |
 |___||_| |_| \____| \___||_| |_|
 
 International Genetics Technologies, Inc.
 === JURASSIC PARK CONTROL SYSTEM - NUBLAR FACILITY ===
 
 WARNING: UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED.
 ALL ACTIVITIES ARE LOGGED AND MONITORED BY SYSTEM ADMIN (R. ARNOLD).
```

---

## 7) ¿Cuál es el hostname?
El hostnanme es: lab-nublar-os. 

```bash
dnedry@lab-nublar-os:~$ hostname
lab-nublar-os
```

---

## 8) Según la información del sistema, ¿en qué tipo de distribución está basado InGenOS?
Esta basado en securityos, ya que mediante el comando `cat /etc/os-release`, obtenemos:

```bash
dnedry@lab-nublar-os:~$ cat /etc/os-release
NAME="InGenOS (Jurassic Park Control System)"
VERSION="1.0.4 (Nublar Edition)"
ID=ingenos
ID_LIKE=securityos
PRETTY_NAME="InGenOS 1.0.4 LTS"
VERSION_ID="1.0.4"
HOME_URL="http://www.jurassicpark.com/"
SUPPORT_URL="http://help.ingen.com/"
BUG_REPORT_URL="http://bugs.ingen.com/"
```

---

## 9) ¿A qué grupos pertenece el usuario?
El usuario pertenece a los grupos: 

```bash
dnedry@lab-nublar-os:~$ id
uid=1000(dnedry) gid=1000(dnedry) groups=1000(dnedry),4(adm),24(cdrom),30(dip),46(plugdev),114(lpadmin),115(sambashare)
```

Saber esto es crucial para definir las capacidades reales que tiene el usuario dentro del sistema.







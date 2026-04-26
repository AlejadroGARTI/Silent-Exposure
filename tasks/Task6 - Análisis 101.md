# Task 6 Análisis del sistema 101

## Información general 
- **Herramientas utilizadas:**  

---

## Investigación Inicial

Se 

---

## 1) ¿Cuántos puertos tiene abierto la máquina?
La máquina tiene abiertos 4 puertos, que se pueden ver mediante ss -tuln: 

| Netid | State  | Recv-Q | Send-Q | Local Address:Port | Peer Address:Port |
|-------|--------|--------|--------|--------------------|-------------------|
| udp   | UNCONN | 0      | 0      | *:68               | *:*               |
| tcp   | LISTEN | 0      | 128    | *:80               | *:*               |
| tcp   | LISTEN | 0      | 128    | *:22               | *:*               |
| tcp   | LISTEN | 0      | 128    | :::22              | :::*              |

---

## 2) ¿Qué ha abierto el protocolo UDP?
El protocolo está abierto en el púerto 68, tal como se puede observar en la tabla de la pregunta 1.

---

## 3) ¿Qué porcentaje de disco está siendo usando por el sistema?
El 

---

## 4) ¿Cuántos bytes hay disponible en el disco del sistema?
El código de error es 

---

## 5) El usuario www-data, ¿qué proceso está ejecutando en el sistema?
Se puede averiguar mediante el comando: ps -u www-data -f, que nos devuelve: 

www-data   677     1  0 00:45 ?        00:00:00 /usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
 
---

## 6) ¿Cuántos binarios con el bit SUID activo existen en el sistema?
Se utilizó el comando: find / -perm -4000 2>/dev/null | wc -l, y se utiliza para buscar todos los archivos del sistema que tienen activado el bit SUID (Set User ID) y contar cuántos existen en total. En este caso, el resultado obtenido es 16, lo que significa que hay 16 binarios en el sistema con el bit SUID habilitado. 

Este permiso especial permite que ciertos programas se ejecuten con los privilegios del propietario del archivo (normalmente root), independientemente del usuario que los ejecute, por tanto, la presencia de estos 16 binarios indica los programas que potencialmente pueden ejecutarse con privilegios elevados y que, en un contexto de seguridad, podrían representar posibles vectores de escalada de privilegios si alguno de ellos está mal configurado o presenta vulnerabilidades.

---

## 7) ¿En que estado se encuentra el firewall?
Al no tener en este momento permisos sudo, se puede ver el estado del firewall mediante systemctl status ufw, en donde observamos: 

- ufw.service - Uncomplicated firewall
- Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
- Active: active (exited) since Sun 2026-04-26 00:45:21 PDT; 2h 4min ago
- Process: 188 ExecStart=/lib/ufw/ufw-init start quiet (code=exited, status=0/SUCCESS)
- Main PID: 188 (code=exited, status=0/SUCCESS)
- CGroup: /system.slice/ufw.service


---

## 8) ¿Qué versión tiene el paquete apt?
Mediante apt --version, vemos que el paquete tiene la versión: apt 1.2.32ubuntu0.1 (amd64)

---

## 9) ¿Qué prioridad tiene el paquete apt para el sistema?
Mediante apt show apt, podemos observar que Priority: important.








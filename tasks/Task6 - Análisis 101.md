# Task 6 Análisis del sistema 101

## Información general 
- **Herramientas utilizadas:**  

---

## Investigación Inicial

Se 

---

## 1) ¿Cuántos puertos tiene abierto la máquina?
La máquina tiene abiertos 4 puertos, que se pueden ver mediante `ss -tuln`: 

| Netid | State  | Recv-Q | Send-Q | Local Address:Port | Peer Address:Port |
|-------|--------|--------|--------|--------------------|-------------------|
| udp   | UNCONN | 0      | 0      | *:68               | *:*               |
| tcp   | LISTEN | 0      | 128    | *:80               | *:*               |
| tcp   | LISTEN | 0      | 128    | *:22               | *:*               |
| tcp   | LISTEN | 0      | 128    | :::22              | :::*              |

---

## 2) ¿Qué ha abierto el protocolo UDP?
El protocolo está abierto en el puerto 68, tal como se puede observar en la tabla de la pregunta 1.

---

## 3) ¿Qué porcentaje de disco está siendo usando por el sistema?
El procentaje se observa mediante `df -h` y es importante conocer este elemento por que el espacio en disco es un recurso finito y crítico cuya saturación puede ser síntoma de un ataque o vector para inhabilitar defensas.

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            463M     0  463M   0% /dev
tmpfs            97M  3.7M   93M   4% /run
/dev/nvme0n1p1   19G  1.5G   17G   9% /
tmpfs           482M     0  482M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           482M     0  482M   0% /sys/fs/cgroup
tmpfs            97M     0   97M   0% /run/user/1000
```

---

## 4) ¿Cuántos kilobytes hay disponible en el disco del sistema?
El espacio disponible en el sistema es de: 

```bash
dnedry@lab-nublar-os:~$ df -a
Filesystem     1K-blocks    Used Available Use% Mounted on
sysfs                  0       0         0    - /sys
proc                   0       0         0    - /proc
udev              475664       0    475664   0% /dev
devpts                 0       0         0    - /dev/pts
tmpfs              99116    3688     95428   4% /run
/dev/nvme0n1p1  19480532 1569688  16895960   9% /
securityfs             0       0         0    - /sys/kernel/security
tmpfs             495560       0    495560   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             495560       0    495560   0% /sys/fs/cgroup
cgroup                 0       0         0    - /sys/fs/cgroup/systemd
pstore                 0       0         0    - /sys/fs/pstore
cgroup                 0       0         0    - /sys/fs/cgroup/perf_event
cgroup                 0       0         0    - /sys/fs/cgroup/pids
cgroup                 0       0         0    - /sys/fs/cgroup/hugetlb
cgroup                 0       0         0    - /sys/fs/cgroup/cpuset
cgroup                 0       0         0    - /sys/fs/cgroup/freezer
cgroup                 0       0         0    - /sys/fs/cgroup/cpu,cpuacct
cgroup                 0       0         0    - /sys/fs/cgroup/net_cls,net_prio
cgroup                 0       0         0    - /sys/fs/cgroup/blkio
cgroup                 0       0         0    - /sys/fs/cgroup/memory
cgroup                 0       0         0    - /sys/fs/cgroup/devices
systemd-1              0       0         0    - /proc/sys/fs/binfmt_misc
fusectl                0       0         0    - /sys/fs/fuse/connections
debugfs                0       0         0    - /sys/kernel/debug
mqueue                 0       0         0    - /dev/mqueue
hugetlbfs              0       0         0    - /dev/hugepages
tmpfs              99116       0     99116   0% /run/user/1000
```

---

## 5) El usuario www-data, ¿qué proceso está ejecutando en el sistema?
Se puede averiguar mediante el comando: `ps -u www-data -f`, que nos devuelve: 

www-data   677     1  0 00:45 ?        00:00:00 /usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
 
---

## 6) ¿Cuántos binarios con el bit SUID activo existen en el sistema?
Se utilizó el comando: `find / -perm -4000 2>/dev/null | wc -l`, y se utiliza para buscar todos los archivos del sistema que tienen activado el bit SUID (Set User ID) y contar cuántos existen en total. En este caso, el resultado obtenido es 16, lo que significa que hay 16 binarios en el sistema con el bit SUID habilitado. 

Este permiso especial permite que ciertos programas se ejecuten con los privilegios del propietario del archivo (normalmente root), independientemente del usuario que los ejecute, por tanto, la presencia de estos 16 binarios indica los programas que potencialmente pueden ejecutarse con privilegios elevados y que, en un contexto de seguridad, podrían representar posibles vectores de escalada de privilegios si alguno de ellos está mal configurado o presenta vulnerabilidades.

---

## 7) ¿En que estado se encuentra el firewall?
Al no tener en este momento permisos sudo, se puede ver el estado del firewall mediante `systemctl status ufw`, en donde observamos: 

```bash
ufw.service - Uncomplicated firewall
    Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
    Active: active (exited) since Sun 2026-04-26 00:45:21 PDT; 2h 4min ago
    Process: 188 ExecStart=/lib/ufw/ufw-init start quiet (code=exited, status=0/SUCCESS)
    Main PID: 188 (code=exited, status=0/SUCCESS)
    CGroup: /system.slice/ufw.service
```

---

## 8) ¿Qué versión tiene el paquete apt?
Mediante `apt --version`, vemos que el paquete tiene la versión: apt 1.2.32ubuntu0.1 (amd64)

---

## 9) ¿Qué prioridad tiene el paquete apt para el sistema?
Mediante `apt show apt`, podemos observar que: (Priority: important).

```bash
dnedry@lab-nublar-os:~$ apt show apt
Package: apt
Version: 1.2.35
Priority: important
Section: admin
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: APT Development Team <deity@lists.debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 3,607 kB
Depends: libapt-pkg5.0 (>= 1.2.35), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2), init-system-helpers (>= 1.18~), ubuntu-keyring, gpgv | gpgv2, gnupg | gnupg2, adduser
Suggests: aptitude | synaptic | wajig, dpkg-dev (>= 1.17.2), apt-doc, python-apt
Breaks: apt-utils (<< 1.1.3), manpages-it (<< 2.80-4~), manpages-pl (<< 20060617-3~), openjdk-6-jdk (<< 6b24-1.11-0ubuntu1~), sun-java5-jdk (>> 0), sun-java6-jdk (>> 0)
Replaces: bash-completion (<< 1:2.1-4.2+fakesync1), manpages-it (<< 2.80-4~), manpages-pl (<< 20060617-3~), openjdk-6-jdk (<< 6b24-1.11-0ubuntu1~), sun-java5-jdk (>> 0), sun-java6-jdk (>> 0)
Task: minimal
Build-Essential: yes
Supported: 5y
Download-Size: 1,107 kB
APT-Sources: http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages
Description: commandline package manager
 This package provides commandline tools for searching and
 managing as well as querying information about packages
 as a low-level access to all features of the libapt-pkg library.
 .
 These include:
  * apt-get for retrieval of packages and information about them
    from authenticated sources and for installation, upgrade and
    removal of packages together with their dependencies
  * apt-cache for querying available information about installed
    as well as installable packages
  * apt-cdrom to use removable media as a source for packages
  * apt-config as an interface to the configuration settings
  * apt-key as an interface to manage authentication keys

N: There are 3 additional records. Please use the '-a' switch to see them.
```







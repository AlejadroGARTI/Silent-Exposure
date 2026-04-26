# Task 7 Escalada de privilegios en Linux

## Información general 
- **Herramientas utilizadas:**  

---

## Investigación Inicial

Mediante la busqueda de información a lo largo del sistema, se pueden recolectar pistas, tales como: 

- Usando `cat /home/dnedry/.bashrc`, como método de inspección para este archivo, al final del mismo tenemos la instrucción (aquí no se encuentra completa, solo la parte importante):

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
Re
Por lo que al inspeccionar este archivo, tenemos el primer dato importante, existe un xploit en tar

---

## 1) ¿Qué pasa cuando se cambia de usuario?
Re

---

## 2) ¿Qué herramienta es vulnerable?
El 

---

## 3) ¿Cuál es la flag de root?
El 

---

## 4) ¿Qué IP ejecutó el proceso?
El código de error es 





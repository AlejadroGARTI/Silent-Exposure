# Task 1 HTTP

## Información General
- **Plataforma:** TryHackMe  
- **IP objetivo:** `10.128.157.53`  
- **Herramientas utilizadas:** Nmap, Navegador web  

---

## Enumeración Inicial

Se realiza un escaneo de servicios para identificar puertos abiertos y versiones:

nmap -sC -sV 10.128.157.53
El comando nmap -sC -sV 10.128.157.53 se utiliza para escanear un objetivo en busca de puertos abiertos y servicios activos. La opción -sC ejecuta scripts básicos de Nmap para obtener información adicional automáticamente, mientras que -sV identifica las versiones de los servicios detectados. Finalmente, la dirección IP indica el host que se está analizando

- 1) ¿A través de que puerto se comunica el servidor?
Resultados relevantes:
PORT   STATE SERVICE VERSION
22/tcp open  ssh
80/tcp open  http    lighttpd 1.4.35
             
El puerto 80/tcp está abierto y ejecuta un servicio HTTP usando el servidor lighttpd 1.4.35.
Esto significa que el objetivo está alojando una página web accesible desde el navegador y al conectarse, el servidor muestra el título “Security Warning Simulation”, lo que indica que probablemente se trata de una página de prueba o un entorno simulado de seguridad.

---

- 2) ¿Qué protocolo se está utilizando para la transferencia de información?
El servidor utiliza el protocolo HTTP (HyperText Transfer Protocol), que es el estándar para la comunicación entre un navegador web y un servidor.

HTTP funciona en el puerto 80 y permite solicitar y recibir páginas web desde el servidor hacia el cliente (navegador), normalmente sin cifrado.

---

- 3) ¿Cúal es el título de la página?

El tìtulo es Security Warning Simulation que se puede ver dentro del comando nmap que se ejecutpo previamente, especìficamente en la linea http-title: Security Warning Simulation.

---

- 4) ¿Cuál es el Error code?
- 5) ¿Cuál es la razón del error?
- 6) ¿Cómo de llama el fichero descargado?
- 7) ¿Cúal es la FLAG?

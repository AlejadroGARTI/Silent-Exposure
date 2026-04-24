# Task 1 HTTP

## Información general
- **Plataforma:** TryHackMe  
- **IP objetivo:** `10.128.157.53`  
- **Herramientas utilizadas:** Nmap, Navegador web  

---

## Investigación Inicial

Se realiza un escaneo de servicios para identificar puertos abiertos y versiones:

nmap -sC -sV 10.128.157.53

El comando nmap -sC -sV 10.128.157.53 se utiliza para escanear un objetivo en busca de puertos abiertos y servicios activos. La opción -sC ejecuta scripts básicos de Nmap para obtener información adicional automáticamente, mientras que -sV identifica las versiones de los servicios detectados. Finalmente, la dirección IP indica el host que se está analizando

---

## 1) ¿A través de qué puerto se comunica el servidor?
Resultados relevantes:
| PORT   | STATE | SERVICE | VERSION 
|--------|-------|---------|---------
| 22/tcp | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 
| 80/tcp | open  | http    | lighttpd 1.4.35 

El puerto 80/tcp está abierto y ejecuta un servicio HTTP usando el servidor lighttpd 1.4.35.
Esto significa que el objetivo está alojando una página web accesible desde el navegador y al conectarse, el servidor muestra el título “Security Warning Simulation”, lo que indica que probablemente se trata de una página de prueba o un entorno simulado de seguridad.

---

## 2) ¿Qué protocolo se está utilizando para la transferencia de información?
El servidor utiliza el protocolo HTTP (HyperText Transfer Protocol), que es el estándar para la comunicación entre un navegador web y un servidor.

HTTP funciona en el puerto 80 y permite solicitar y recibir páginas web desde el servidor hacia el cliente (navegador), normalmente sin cifrado.

---

## 3) ¿Cuál es el título de la página?

El título es Security Warning Simulation que se puede ver dentro del comando nmap que se ejecutó previamente, específicamente en la línea http-title: Security Warning Simulation.

---

## 4) ¿Cuál es el Error code?

El código de error es SEC_ERROR_UNKNOWN_ISSUER.
Este error se puede observar al acceder desde el navegador a la dirección http[:]//192.168.1.50.
Una vez dentro de la página, el navegador muestra el aviso “Warning: Potential Security Risk Ahead”.
Al acceder a la sección Advanced, se puede visualizar el error específico.

---

## 5) ¿Cuál es la razón del error?

Al igual que en el punto 4, esta información se puede observar en la sección Advanced

- Host: 10.128.157.53
- Port: 443
- Reason: Self-signed / untrusted certificate

La explicación es que el host 10.128.157.53 está utilizando el puerto 443, que corresponde al tráfico HTTPS (cifrado). Sin embargo, el motivo del aviso de seguridad es que el servidor presenta un certificado digital auto-firmado o no confiable. Esto significa que el certificado no ha sido emitido por una autoridad de certificación reconocida, por lo que el navegador no puede verificar la identidad del servidor, generando una alerta de seguridad.

---

## 6) ¿Cómo se llama el fichero descargado?

El fichero descargado puede obtenerse al acceder a la opción Advanced y seleccionar “Accept the Risk and Continue”, lo que permite entrar en la página pese a las advertencias del navegador. Este tipo de acciones debe realizarse únicamente en entornos controlados, como máquinas virtuales de pruebas o laboratorios de ciberseguridad. Al continuar, la página descarga automáticamente un fichero, lo cual puede ser peligroso en un entorno real, ya que podría tratarse de software malicioso, scripts o archivos diseñados para explotar vulnerabilidades del sistema, comprometer la seguridad del equipo o ejecutar acciones sin el consentimiento del usuario.

---

## 7) ¿Cuál es la FLAG?

La FLAG es Bienvenidos a Jurassic Park, que se puede observar en la página, a la cual se ha accedido mediante los pasos ejecutados previamente.

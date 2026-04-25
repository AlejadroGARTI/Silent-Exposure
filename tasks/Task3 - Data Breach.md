# Task 3 Análisis de brecha de datos (Data Breach)

## Información general 
- **Herramientas utilizadas:**  

---

## Investigación Inicial

Se analizan el primer fichero encontrado en el paso anterior.
- Para config.php.bak usamos el comando: curl http[:]//10.130.183.40/config.php.bak, que en este caso transfiere los datos a través del protocolo HTTP desde la página web. Mostrando de esta forma toda la información del fichero.

---

## 1) ¿Qué principio de la tríada CIA+ se ve afectado por un Data Breach de información?
Se ve afectada la Confidencialida, ya que implica un acceso no autorizado a información sensible medainte la lectura de ambos ficheros, aunque también puede comprometer la Integridad si los datos se modifican y la Disponibilidad si se destruyen o cifran con ransomware.

---

## 2) ¿Cómo se llama la intranet?
La intranet es una red privada interna de una organización, accesible solo para empleados autorizados, que en este caso se llama: ingen_park_ops.

Este y otros datos listados más adelante se pueden ver en formato de texto plano de la siguiente manera: 

// ** Server settings - Main Park Operations ** //

/** The name of the for InGen Intranet */
define('NAME', 'ingen_park_ops');

/** Server username */
define('USER', 'dnedry');

/** Security password */
define('PASSWORD', 'Wht3_Rbt_0bj_1993!');

/** hostname */
define('HOST', 'localhost');

etc...


---

## 3) ¿Cómo se llama el host?
El 

---

## 4) ¿Qué codificación se utiliza para las tablas?
El código de error es 

---

## 5) Cuál es la AUTH_KEY del sistema?
Al 

---

## 6) ¿Cuál es la SECURE_AUTH_KEY del sistema?
El 

---

## 7) ¿Cuál es el prefijo de la Base de datos?
La 

---

## 8) ¿Qué fichero contiene las clave del parque?
La 


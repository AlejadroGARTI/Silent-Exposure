# Task 3 Análisis de brecha de datos (Data Breach)

## Información general 
- **Herramientas utilizadas:**  Herramienta de enumeración y extracción de información: curl

---

## Investigación Inicial

Se analiza el primer fichero encontrado en el paso anterior, por lo que para config.php.bak usamos el comando: 

`curl http[:]//10.130.183.40/config.php.bak`

En este caso transfiere los datos a través del protocolo HTTP desde la página web. Mostrando de esta forma toda la información del fichero.

```bash
curl http://10.130.178.202/config.php.bak 
<?php
/**
 * The base configuration for the InGen Intranet
 *
 * WARNING: UNAUTHORIZED ACCESS TO THIS FILE IS STRICTLY PROHIBITED.
 * Dennis, I've told you three times to stop leaving backup files 
 * in public directories. Secure this immediately! - Ray Arnold
 *
 * This file contains the following configurations:
 *
 * * Settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @package InGen_Intranet
 */

// ** Server settings - Main Park Operations ** //
/** The name of the for InGen Intranet */
define('NAME', 'ingen_park_ops');

/** Server username */
define('USER', 'dnedry');

/** Security password */
define('PASSWORD', 'Wht3_Rbt_0bj_1993!');

/** hostname */
define('HOST', 'localhost');

/** Charset to use in creating tables. */
define('CHARSET', 'utf8mb4');

/** The Collate type. Don't change this if in doubt. */
define('COLLATE', '');

/**#@+
 * Authentication Unique Keys and Salts.
 *
 * InGen Security protocols require these to be completely random.
 *
 * @since 1993.0
 */
define('AUTH_KEY',         '&D5bY9KL_lD(GDIePp*o&^~K');
define('SECURE_AUTH_KEY',  'NR;@e/,/!cqzLP@+`C');
define('LOGGED_IN_KEY',    ']2,WH2?ObnBe{1dSO#lS0~GnM/FL');
define('NONCE_KEY',        'N{OuK9:[GVte*?6=%3OsNKjz1P=O*vL5');
define('AUTH_SALT',        'tF}YpjS^OkC%xP^NEt|PlNP4bTdr');
define('SECURE_AUTH_SALT', 'IYl(@rblISw?to/DR');
define('LOGGED_IN_SALT',   '!+:WvG7k4%NC3bq!1oqXK{');
define('NONCE_SALT',       'b(DU{{YDRgEk3})&]I0N`egT7bEJCPymX.');
/**#@-*/

/**
 * InGen Database Table prefix.
 *
 * You can have multiple installations in one database if you give each
 * a unique prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'ingen_';

/**
 * For developers: Debugging mode.
 * * Ray, leave this on true while I fix the vending machines. 
 * I am compiling some code and need to see the errors. - Nedry
 */
define('WP_DEBUG', true);

/* That's all, stop editing! Welcome to Jurassic Park. */

/** Absolute path to the intranet directory. */
if ( !defined('ABSPATH') ) {
        define('ABSPATH', dirname(__FILE__) . '/');
}

/** Sets up park keys and included files. */
require_once(ABSPATH . 'public_key.asc');
```

---

## 1) ¿Qué principio de la tríada CIA+ se ve afectado por un Data Breach de información?
Se ve afectada la Confidencialidad, ya que implica un acceso no autorizado a información sensible mediante la lectura de ambos ficheros, aunque también puede comprometer la Integridad si los datos se modifican y la Disponibilidad si se destruyen o cifran con ransomware.

---

## 2) ¿Cómo se llama la intranet?
La intranet es una red privada interna de una organización, accesible solo para empleados autorizados, que en este caso se llama: ingen_park_ops.

Este y otros datos listados más adelante se pueden ver en formato de texto plano de la siguiente manera: 

```bash
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
```

---

## 3) ¿Cómo se llama el host?
El host es el dispositivo conectado a una red, identificado por una IP o nombre, por lo que en este caso `localhost` es el propio equipo en donde se ejecuta el software.

---

## 4) ¿Qué codificación se utiliza para las tablas?
Se usa `utf8mb4` y es un juego de caracteres que soporta todos los caracteres Unicode, incluyendo emoticonos; es la recomendada para MySQL por su mayor compatibilidad.

---

## 5) Cuál es la AUTH_KEY del sistema?
Esta es la clave criptográfica única que se usa en WordPress para cifrar cookies de sesión y autenticación, aumentando la seguridad contra ataques de suplantación. La clave sería: **`&D5bY9KL_lD(GDIePp*o&^~K`** y en este caso, AUTH_KEY probablemente protege las cookies de autenticación normales del inicio de sesión estándar.

---

## 6) ¿Cuál es la SECURE_AUTH_KEY del sistema?
La definición sería igual que la AUTH_KEY, solo que en este caso la SECURE_AUTH_KEY probablemente protege, específicamente, a las cookies enviadas por HTTPS y estaría dada por: **"NR;@e/,/!cqzLP@+`C**

---

## 7) ¿Cuál es el prefijo de la Base de datos?
El prefijo es una cadena añadida al inicio de cada tabla para identificar las tablas de una aplicación específica dentro de una misma BD, permitiendo múltiples sistemas en una sola base, en este caso estaría dada por: ingen_

---

## 8) ¿Qué fichero contiene las claves del parque?
EL fichero con claves es un archivo que almacena las claves de seguridad, credenciales de BD y configuraciones sensibles del sistema, y estaría dado por: public_key.asc

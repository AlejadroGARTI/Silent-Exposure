# Task 4 Análisis de claves GPG (GNU Privacy Guard)

## Información general 
- **Herramientas utilizadas:** GNU Privacy Guard y GNU Wget 

---

## Investigación Inicial
1) Descargamos el fichero mediante: `wget http[:]//10.130.183.40/security.zip`
2) Lo descomprimimos usando la clave recuperada del fichero config.php.bak
3) Obtenemos la información mediante: `gpg --import --import-options show-only public_key.asc`

```bash 
┌──(kali㉿kali)-[~]
└─$ gpg --import --import-options show-only public_key.asc
gpg: keybox '/home/kali/.gnupg/pubring.kbx' created
gpg: /home/kali/.gnupg/trustdb.gpg: trustdb created
pub   rsa3072 2026-04-18 [SC]
      1E8176A69B558BAB0D059E87032F03187B650702
uid                      Dennis Nedry (La key del Lab: L3Dodgson5 <dnedry@ingen.com>) <dnedry@ingen.com>
sub   rsa3072 2026-04-18 [E]
```

---

## 1) ¿Cuál es la contraseña del fichero?
La contraseña del fichero es "Wht3_Rbt_0bj_1993!", la cual fue recuperada de config.php.bak, lo que demuestra un grave problema de seguridad, ya que los únicos ficheros disponibles comparten credenciales, permitiendo el acceso entre ellos sin necesidad de otras herramientas.

---

## 2) ¿En qué está basado el fichero que se ha obtenido?
Está basado en Estándar ASCII, el cual es un código de caracteres que asigna números del 0 al 127 a letras, dígitos, signos de puntuación y controles, permitiendo que computadoras y dispositivos intercambien texto de forma universal.

---

## 3) ¿Cuál es el inicio de la clave pública?
El inicio de la clave pública se puede visualizar mediante `less public_key.asc`, en donde se puede observar: 

```bash
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQGNBGnjWv4BDACqYvlAtc9o3Lve1YOUvgHphHCKyCLIy2swF1PDE0FR39n15H01
PiJ4i/60nQzU6UG+kiRO3p2D6TUzXVK+3acnaO9hc....
```

Una public key o clave pública es una clave criptográfica que forma parte de la criptografía asimétrica y puede compartirse libremente con otras personas y su función principal es permitir que otros cifren mensajes destinados a ti o verifiquen firmas digitales que se hayan creado con una clave privada. Aunque cualquiera puede usarla, solo la clave privada correspondiente puede descifrar la información o generar la firma válida.

---

## 4) ¿Qué algoritmo de cifrado utiliza la clave pública "Public Key"?
La clave pública utiliza el algoritmo de cifrado rsa3072. 

En este caso, el 3072 indica el tamaño de la clave en bits, lo que significa que es una clave muy robusta y segura para cifrar información o firmar digitalmente.

---

## 5) ¿Cuándo fue creada?
Fue creada el 18/04/2026.

pub: rsa3072 2026-04-18 [SC]

---

## 6) ¿A quién pertenece?
Pertenece a Dennis Nedry

uid: Dennis Nedry

---

## 7) ¿Cuál es su correo?
El correo asociado es el que está vinculado a esa identidad dentro de la clave pública: dnedry@ingen[.]com



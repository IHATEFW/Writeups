La máquina Tomghost de la plataforma TryHackMe.com es una máquina de dificultad "Fácil", que nos enseña a identificar y explotar una vulnerabilidad reciente en un servicio real. En este caso, nos toparemos con un servidor Apache Tomcat afectado por la famosa vulnerabilidad "Ghostcat" (CVE-2020-1938), que abusa del protocolo AJP para leer archivos internos de la aplicación, desde donde filtraremos unas credenciales. Posteriormente, descifraremos una clave PGP privada crackeando su passphrase con John The Ripper para pivotar entre usuarios y, finalmente, escalaremos a root aprovechando un binario mal configurado en el sudoers (zip) con ayuda de GTFOBins . .

# TOMGHOST

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1853" height="909" alt="tom1" src="https://github.com/user-attachments/assets/056a2dd2-0a06-46f1-86cf-4ea4f93f22fb" />

<img width="1853" height="909" alt="tom2" src="https://github.com/user-attachments/assets/87ec0a5e-2c92-4c2c-bbbd-5300704d93f0" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente está encendida. A su vez le lanzamos un ping con la opción -R (Record Route) que nos indica que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64, que nos indica que nos enfrentamos a una máquina linux.

<img width="1853" height="909" alt="tom3" src="https://github.com/user-attachments/assets/fd96a34d-b497-4096-85b0-778eb62c3b17" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima. Lanzaremos un escaneo a todo el rango de puertos de la siguiente manera, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 (SSH), 53 (domain), 8009 (ajp13) y 8080 (http-proxy).

<img width="1767" height="683" alt="tom4" src="https://github.com/user-attachments/assets/d4912bad-9a0d-4661-86cc-d73060e71d4f" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios. Al ejecutarlo, podemos visualizar que el puerto 22 corre OpenSSH 7.2p2 sobre un Ubuntu, el puerto 8009 corre el protocolo AJP (Apache Jserv Protocol v1.3) y, lo más interesante, el puerto 8080 corre un Apache Tomcat en su versión 9.0.30.

<img width="1798" height="844" alt="tom5" src="https://github.com/user-attachments/assets/9457d1ae-fb24-4e00-9a93-94086f17c343" />

Abriremos el navegador y visitaremos la web detrás del puerto 8080, confirmamos que efectivamente es la página por defecto de un Apache Tomcat 9.0.30, un gestor de aplicaciones web escrito en Java.

<img width="1803" height="892" alt="tom6" src="https://github.com/user-attachments/assets/3c075a92-0200-46fe-ad9a-0ab38da78621" />

Como ya conocemos la versión exacta del servicio, buscaremos en internet si existe alguna vulnerabilidad conocida para dicha versión. Buscamos "Apache Tomcat/9.0.30 exploit github" y nos encontramos con la famosa vulnerabilidad "Ghostcat" (CVE-2020-1938), que afecta al protocolo AJP (puerto 8009) y permite leer/incluir archivos internos de la aplicación sin autenticación.

<img width="1124" height="892" alt="tom7" src="https://github.com/user-attachments/assets/2d65d200-78b0-4eb6-8681-d3d921ed5565" />

Verificamos también desde nuestra terminal con la herramienta searchsploit, que busca exploits en la base de datos local de Exploit-DB, y efectivamente nos muestra el exploit "Apache Tomcat - AJP 'Ghostcat' File Read/Inclusion", confirmando que la máquina es vulnerable.

<img width="1766" height="420" alt="tom8" src="https://github.com/user-attachments/assets/4fa89d3f-abba-4754-8eff-3ef1b6342162" />

## 💣 EXPLOTACIÓN

Para explotar la vulnerabilidad Ghostcat utilizaremos Metasploit, un framework de explotación muy utilizado. Lo levantamos con el comando msfconsole.

<img width="1307" height="823" alt="tom9" src="https://github.com/user-attachments/assets/0c649346-ba85-4918-a0c4-c16d1ffe3d91" />

Dentro de Metasploit, buscamos el módulo con search ghostcat, lo seleccionamos con use 0 (auxiliary/admin/http/tomcat_ghostcat) y revisamos sus opciones con options. Vemos que por defecto intentará leer el archivo /WEB-INF/web.xml a través del puerto 8009 (AJP), solo nos falta indicarle el objetivo.

<img width="1671" height="823" alt="tom10" src="https://github.com/user-attachments/assets/4fbb15ec-11d0-4c3a-9736-61c9878b2050" />

Le indicamos la dirección ip de la máquina víctima con set RHOSTS y ejecutamos el módulo con run. El módulo abusa del AJP y nos devuelve el contenido del archivo web.xml que se encontraba protegido.

<img width="1671" height="823" alt="tom11" src="https://github.com/user-attachments/assets/094e9481-a7a1-4fed-9d83-f71a3e87c790" />

Al leer el contenido del archivo web.xml, nos encontramos con una sorpresa dentro del apartado description: unas credenciales en texto plano del usuario skyfuck con su contraseña 8730281lkjlkjdqlksalks.

<img width="1671" height="823" alt="tom12" src="https://github.com/user-attachments/assets/4517dcc5-dcde-4bfe-a01e-0b5c6e0568b7" />

Como el puerto 22 (SSH) estaba abierto, probaremos a conectarnos vía SSH con las credenciales que acabamos de filtrar. ¡Logramos acceso a la máquina víctima como el usuario skyfuck!

<img width="1671" height="823" alt="tom13" src="https://github.com/user-attachments/assets/182af2f0-4120-4314-9263-b1d0bf13adb6" />

Ya dentro de la máquina, procederemos a enumerar los usuarios del sistema leyendo el archivo /etc/passwd. Nos damos cuenta de que, además de skyfuck, existen los usuarios merlin y tomcat con una shell válida (/bin/bash).

<img width="1254" height="823" alt="tom14" src="https://github.com/user-attachments/assets/77bb226c-e444-47b0-a267-0d5c2c877bc7" />

Buscaremos la primera flag user.txt por todo el sistema con el comando find, encontramos que se ubica en el directorio del usuario merlin (/home/merlin/user.txt), la leemos y copiamos la flag.

<img width="1214" height="374" alt="tom15" src="https://github.com/user-attachments/assets/1632a6c1-6f11-4a48-9710-e6e54938b229" />

Pegamos la flag en la respuesta de TryHackMe correspondiente a "Compromise this machine and obtain user.txt", le damos enter y nos la marca como correcta.

<img width="1854" height="901" alt="tom16" src="https://github.com/user-attachments/assets/d99ac5a3-8c85-45ac-9aea-3e42f3c79a76" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ahora que ya tenemos la flag de usuario, nuestro objetivo es convertirnos en root. Enumerando el directorio home del usuario skyfuck, nos encontramos con dos archivos muy interesantes: credential.pgp (un archivo cifrado) y tryhackme.asc (una clave privada PGP). Leemos el contenido de la clave privada.

<img width="872" height="901" alt="tom17" src="https://github.com/user-attachments/assets/8297dece-cc3b-462e-8676-5ce9e3c71bb4" />

Si intentamos descifrar directamente el archivo credential.pgp con gpg --decrypt, nos pide una passphrase (frase de paso) para desbloquear la clave secreta del usuario tryhackme, la cual no tenemos. Por lo tanto, tendremos que crackearla.

<img width="771" height="605" alt="tom18" src="https://github.com/user-attachments/assets/e5f9d408-b544-4b80-96c6-2764b0a675a9" />

Para crackear la passphrase, primero nos copiaremos el contenido de la clave privada PGP a nuestra máquina atacante en un archivo que llamaremos prueba.asc.

<img width="1012" height="874" alt="tom19" src="https://github.com/user-attachments/assets/179f2190-dab5-42a5-8f0e-8a33f6b3b3b1" />

Con la herramienta gpg2john, convertiremos la clave privada a un formato de hash que John The Ripper pueda entender, y lo guardamos en un archivo llamado hash.txt.

<img width="1183" height="309" alt="tom20" src="https://github.com/user-attachments/assets/77f8134c-bda3-4652-8ba3-99e87c4255b9" />

Ahora lanzaremos un ataque de fuerza bruta/diccionario con John The Ripper utilizando el famoso diccionario rockyou.txt. En cuestión de segundos crackea la passphrase: alexandru.

<img width="1728" height="521" alt="tom21" src="https://github.com/user-attachments/assets/8aff6437-a122-4eaa-a763-f6d1adbbf760" />

Volvemos a la máquina víctima y desciframos el archivo credential.pgp con gpg --decrypt, esta vez introduciendo la passphrase alexandru que acabamos de crackear. El archivo nos revela las credenciales del usuario merlin. Con dichas credenciales hacemos un su merlin para cambiar de usuario y revisamos sus permisos sudo con sudo -l. ¡Encontramos que merlin puede ejecutar el binario /usr/bin/zip como root y sin contraseña (NOPASSWD)! Nos dirigimos a la web gtfobins.org, filtramos por zip y, en el apartado de Sudo, copiamos el comando que nos permite abusar de este binario para obtener una shell como root.

<img width="1356" height="846" alt="tom22" src="https://github.com/user-attachments/assets/d577cbb2-03c9-4e71-8a7d-29535e2e9da4" />

Ejecutamos el comando de GTFOBins y ¡somos root!, máquina hackeada con éxito. Nos dirigimos al directorio /root y encontramos la última flag root.txt

<img width="913" height="439" alt="tom23" src="https://github.com/user-attachments/assets/48f145ac-f454-4b1e-a19b-33e3d22c367c" />

Pegamos la última flag en la web y nos aparece el mensaje de finalización. ¡Máquina Tomghost completada con éxito!

<img width="1854" height="901" alt="tom24" src="https://github.com/user-attachments/assets/f0b2a73c-9ad7-45db-b67c-980ccfca2ddb" />

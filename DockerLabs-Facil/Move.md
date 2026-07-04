La máquina Move de la plataforma DockerLabs.es, es una máquina de dificultar "Fácil", la cual nos enseña que existe una versión vulnerable de Grafana, la 8.3.0, la cual nos permite leer archivos del sistema, una vez leemos archivos sensibles, procedemos a ganar acceso a la máquina, finalmente con privilegios a nivel de sudoers pivotamos a root.-


# MOVE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="799" height="504" alt="move1" src="https://github.com/user-attachments/assets/87b6ff41-4376-48eb-85c1-dc87d582d364" />

## 🔎 ENUMERACIÓN

El primer paso para comprometer esta máquina, será enumerar los puertos con la herramienta nmap, esto para lograr identificar posibles puertos abiertos/expuestos que tenga la máquina víctima, una vez ejecutado el primer escaneo, vemos que se visualizan tres puertos abiertos, el 22, 80 y 3000, correspondientes a SSH, HTTP y el último puerto se utiliza por defecto para entornos de desarrollo de software.

<img width="886" height="638" alt="move2" src="https://github.com/user-attachments/assets/0b1fd2de-fee4-475e-a7d1-1527e58eb80d" />

Una vez ya tenemos identificados los puertos abiertos, procederemos a seguir enumerando con nmap, pero esta vez, realizaremos un escaneo más exhaustivo, indicandole a nmap que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos encuentre la versión de dichos servicios, una vez ejecutado, podemos visualizar que está corriendo Grafana en el puerto 3000, vamos a revisarlo.

<img width="1041" height="638" alt="move3" src="https://github.com/user-attachments/assets/8330288a-14e6-4aab-b3ac-5f1955956243" />

Pero antes de revisarlo, vamos a realizar fuerza bruta de directorios con la herramienta GoBuster, esto con la finalidad de encontrar posibles directorios ocultos despues de la /, una vez ejecutado, visualizamos que nos encontró un directorio maintenance.html, lo procederemos a revisar.-

<img width="1146" height="554" alt="move4" src="https://github.com/user-attachments/assets/8a9ebaf9-2fc4-4d8c-a8d6-ec5e0c665205" />

Vemos que nos indica una pista, revisar el archivo /tmp/pass.txt, lo intentamos colar en la url, pero no deja de ninguna manera.

<img width="1076" height="331" alt="move5" src="https://github.com/user-attachments/assets/0aa94fdd-58c1-4a8a-a678-b16e4199b5c4" />

Ahora si revisamos el Grafana que está en el puerto 3000, y nos muestra el típico login inicio de sesión, pero abajo, nos muestra la versión de dicho Grafana, la 8.3.0

<img width="1214" height="638" alt="move6" src="https://github.com/user-attachments/assets/92eb2571-0f33-415b-8bef-88fd2693cf05" />

En este punto, procederemos a buscar por la terminal con la herramienta searchsploit que utiliza la BDD de exploit-db, para ver si existen posibles exploits para Grafana 8.3.0, encontramos un script .py que es un vulnerable a "Directory Traversal and Arbitrary File Read", lo cual nos permitirá leer archivos del sistema. 

<img width="1330" height="331" alt="move7" src="https://github.com/user-attachments/assets/9f521ff7-c216-4b22-b19a-9738baca2183" />

## 💣 EXPLOTACIÓN

Lo descargamos de la siguiente manera.

<img width="780" height="630" alt="move8" src="https://github.com/user-attachments/assets/0488b912-58cf-4bcf-8386-9e1cf22f0bfe" />

Y lo ejecutamos, de inmediato vamos a leer la pista que nos dejaron, el /tmp/pass.txt, encontrando una posible contraseña, luego vamos a leer el archivo /etc/passwd para identificar usuarios válidos y encontramos el usuario "freddy".

<img width="780" height="630" alt="move9" src="https://github.com/user-attachments/assets/af848510-ef53-463d-af03-ff2357da863f" />

Nos logueamos por SSH como freddy, y ¡Ganamos acceso a la máquina víctima!

<img width="857" height="514" alt="move10" src="https://github.com/user-attachments/assets/6201d938-7ea4-4559-b06d-88935ca0ad09" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina víctima, daremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers para poder ejecutar algun binario y vemos que si, podemos ejecutar un script .py como el usuario root.

<img width="949" height="341" alt="move11" src="https://github.com/user-attachments/assets/c8f30c0e-3399-49d3-a7b8-8a1070a869d6" />

Finalmente la revisamos, y le ingresamos código malicioso de python para que nos lance una shell como root, la ejecutamos y ¡Ya somos root!, máquina hackeada.-

<img width="553" height="534" alt="move12" src="https://github.com/user-attachments/assets/86e7f643-1e86-46a6-81b4-a1e2efb80db2" />

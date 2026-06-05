Esta máquina llamada Picadilly de la plataforma DockerLabs.es, es de dificultad fácil, la cual tiene expuesto el puerto 443 que nos permite subir un archivo malicioso, en este caso una reverse shell en php, del cual nos aprovechamos para ganar acceso a la máquina víctima . .

# PICADILLY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="851" height="589" alt="picadilly1" src="https://github.com/user-attachments/assets/ad8bfd69-1f16-4a2c-b489-cc1df7d5ec31" />

## 🔎 ENUMERACIÓN

El primer paso que realizaremos, es la enumeración, en este caso utilizaremos la herramienta nmap para enumerar todos los puertos que tenga abierta la máquina víctima, con el siguiente comando, una vez ejecutado, podemos visualizar que exísten los puertos 80 y 443 expuestos, correspondientes a los servicios HTTP y HTTPS.

<img width="881" height="631" alt="picadilly2" src="https://github.com/user-attachments/assets/39c68dae-5b26-495a-bb2e-db26d6e92282" />

Seguíremos enumerando con la herramienta nmap, pero esta vez indicandole a nmap que nos arroje la versión de dichos servicios, a su vez, tambien le indicaremos que nos ejecute un conjunto básico de scripts de reconocimiento, esto con el fin de encontrar más información que nos pueda servir, una vez ejecutado, podemos visualizar que exíste un achivo llamado backup.txt, tambien podemos visualizar que exíste una web llamada Picadilly detrás del puerto 443, vamos a revisar ambas.

<img width="844" height="647" alt="picadilly3" src="https://github.com/user-attachments/assets/e25c91b6-b4e6-46a6-b407-a2ff6c83824a" />

Ingresamos al archivo backup.txt y podemos visualizar que exíste un mensaje encriptado, aparentemente la contraseña del usuario mateo, con un mensaje que dice "Para resolver este enigma, piensa en un antiguo emperador romando y su sencillo método de cambiar letras", de primera ya nos damos cuenta que hace referencia al cifrado César.

<img width="1143" height="504" alt="picadilly4" src="https://github.com/user-attachments/assets/ec00a7ba-ab70-4d0d-a836-79bb07ee9b24" />

Nos copiamos el mensaje encriptado, y en google buscamos alguna web que nos haga decoding en cifrado César de dicho mensaje:

<img width="1197" height="643" alt="picadilly5" src="https://github.com/user-attachments/assets/cfa23112-3466-4379-b091-2c0aa01ef422" />

Ya tenemos la contraseña, la guardamos para déspues, ahora procederemos a revisar la web detrás del puerto 443, vemos que la web nos deja subir un archivo, abrimos las herramientas del desarrollado o también llamado inspector para ver donde se guardaría dicho archivo, vemos un directorio /uploads.php, perfecto, en este punto ya sabemos que deberemos subir una reverse shell .php

<img width="1241" height="643" alt="picadilly6" src="https://github.com/user-attachments/assets/3fc8d1ac-b443-4bcc-8709-107c527646b1" />

<img width="819" height="654" alt="picadilly7" src="https://github.com/user-attachments/assets/c242dc85-63d5-42b9-9475-383b848293ef" />

## 💣 EXPLOTACIÓN

Comenzamos la explotación, de inmediato nos dirigiremos a la web revshells.com para escoger la reverse shell que más nos acomode, yo escogí la Pentest Monkey de PHP, la configuramos con nuestra ip atacante y el puerto que queramos ponernos en escucha:

<img width="1191" height="643" alt="picadilly8" src="https://github.com/user-attachments/assets/8829be5a-9b72-4ba3-a0e2-015c2d89b221" />

La guardamos en un archivo .php en nuestro directorio, la subiremos a la web y nos ponemos en escucha con netcat con el siguiente comando, nos dirigimos al directorio /uploads y le damos click, volvemos a la terminal y vemos que ya tenemos acceso a la máquina víctima.

<img width="962" height="308" alt="picadilly9" src="https://github.com/user-attachments/assets/f8b5169a-2048-4589-835d-c254fd52b010" />

<img width="896" height="571" alt="picadilly10" src="https://github.com/user-attachments/assets/e62b519f-fbfc-44f7-b707-0def11b7dc31" />

<img width="1204" height="376" alt="picadilly11" src="https://github.com/user-attachments/assets/d7dc87a9-0af1-4be6-bf47-aad2c1427ce0" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash

<img width="545" height="169" alt="picadilly12" src="https://github.com/user-attachments/assets/c3b3ed40-5b68-4a4d-b78e-76c490fa3e55" />

<img width="562" height="238" alt="picadilly13" src="https://github.com/user-attachments/assets/430533e2-7773-4cd6-ab4d-9aa2a93fd3d1" />

Ya con una tty estable, vamos a leer el archivo /etc/passwd para ver si mateo es el único usuario o exísten más, vemos que mateo es el unico usuario que tiene /bin/bash, por lo tanto, utilizaremos la password que decodeamos con Cifrado César hace un rato, y nos convertimos al usuario mateo.

<img width="1222" height="639" alt="picadilly14" src="https://github.com/user-attachments/assets/578afcd1-9902-4d90-8a5e-f194ca3df103" />

Ya con el usuario mateo, daremos el comando sudo -l para ver si podemos ejecutar algun binario con privilegios a nivel de sudoers, y vemos que efectivamente podemos ejecutar php como el usuario root, procederemos a recurrir a la web gtfobins.org y filtramos por php, no dirigimos al apartado "Sudo" y utilizamos el comando que aparece para subir a root, máquina hackeada.

<img width="662" height="135" alt="picadilly15" src="https://github.com/user-attachments/assets/e8ca1f3d-5719-4aa7-99b0-f09e6e815394" />

<img width="1106" height="639" alt="picadilly16" src="https://github.com/user-attachments/assets/0ab5facc-bb2b-4603-941e-a68e0e048b03" />

<img width="578" height="242" alt="picadilly17" src="https://github.com/user-attachments/assets/26eb06b8-58bf-4c99-9396-8b3407b128fb" />


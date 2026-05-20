Quinta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", 

# BREAKMYSSH

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1514" height="374" alt="breakmyssh1" src="https://github.com/user-attachments/assets/ee9f3a85-d972-4695-8e19-53ef796453e6" />

## 🔎 ENUMERACIÓN

Comenzamos enumerando los puertos abiertos con la herramienta nmap, esto nos permitirá dar el primer paso para comprometer y tantear el terreno de cara a comprometer la máquina, lanzaremos el siguiente el comando destacado en la evidencia, el cual nos arrojó que existe el puerto 22 abierto correspondiente al servicio SSH:

<img width="1503" height="785" alt="breakmyssh2" src="https://github.com/user-attachments/assets/c14fe5e1-aaaa-40f2-b8ff-fbaa640332d9" />

Como ya sabemos que existe el puerto 22 abierto, seguiremos enumerando con la herramienta nmap de manera más exhaustiva, ahora indicandole a nmap que nos detecte la version de ese servicio SSH, además de indicarle que nos arroje un conjunto básico de scripts de reconocimiento:

<img width="1517" height="786" alt="breakmyssh3" src="https://github.com/user-attachments/assets/b37dab37-7675-469b-8828-6026be6b4788" />

Vemos que nos encontró la versión 7.7 de OpenSSH, la cual es vulnerable a una enumeración de usuarios, si buscamos con el comando "searchsploit ssh 7.7", encontraremos un exploit que nos permitirá enumerar usuarios válidos dentro de la máquina víctima, esto ya está parchado, y solo es útil en esa versión, quiere decir que posterior a la 7.7 ya no aplica, por lo tanto, utilizaremos metasploit para utilizar dicho exploit y encontrar algun usuario válido:

## 💣 EXPLOTACIÓN

Aquí está la evidencia que existen exploits para esta versión vulnerable de OpenSSH 7.7:

<img width="1519" height="297" alt="breakmyssh4" src="https://github.com/user-attachments/assets/f312bc41-c306-4c94-abd1-724c0820cfc2" />

Abriremos msfconsole, ejecutaremos el comando "searchsploit ssh enumerate" y seleccionaremos el exploit con dígito "1" con el comando "use 1", ejecutaremos options para ver lo que nos piden configurar, en este caso el RHOSTS que es la ip de la máquina víctima y el USER_FILE que le otorgaremos serán 2 diccionarios de usuarios básicos que es el top-usernames-shortlists.txt y el xato-net-10-million-usernames.txt, uno primero y el otro despues, finalmente le daremos run, con el primero encontramos el usuario root y con el segundo el usuario lovely.

<img width="1521" height="784" alt="breakmyssh5" src="https://github.com/user-attachments/assets/15c6f238-2893-4d24-b643-deb143744054" />

<img width="1525" height="784" alt="breakmyssh6" src="https://github.com/user-attachments/assets/a8aa32b7-4063-49fd-955c-5a92e0995ee9" />

Una vez ya encontramos el usuario válido lovely, ejecutaremos un ataque de fuerza bruta SSH con hydra, con el siguiente comando, encontramos la contraseña correspondiente y accedemos por SSH, ¡Ganamos acceso a la máquina víctima!

<img width="1528" height="492" alt="breakmyssh7" src="https://github.com/user-attachments/assets/aa8c19a1-a7a7-400a-889e-d241ad8a3844" />

## 🔑 ESCALADA DE PRIVILEGIOS

Por último, para escalar privilegios intentaremos probar con el comando "sudo -l" para ver si tenemos permisos a nivel de sudoers, pero no existe el comando "sudo", empezaremos a buscar en directorios típicos como /opt, le damos ls -ltra para buscar archivos ocultos y encontramos un archivo .hash, lo leemos y nos muestra un hash misterioso, nos dirigiremos a la página hashes.com para descodear el hash, y encontramos la password de root, máquina hackeada . .

<img width="888" height="361" alt="breakmyssh8" src="https://github.com/user-attachments/assets/634f734a-351e-4a4a-a8d0-430490c4262b" />

<img width="1036" height="620" alt="breakmyssh9" src="https://github.com/user-attachments/assets/542c884d-23ae-4e3d-851c-a8c517cbcf5f" />

<img width="315" height="101" alt="breakmyssh10" src="https://github.com/user-attachments/assets/5c09d5ba-2e55-49b2-acc2-f674bba6bcc9" />



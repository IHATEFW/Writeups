<img width="1905" height="376" alt="image" src="https://github.com/user-attachments/assets/10e5c33c-a510-4630-84b9-f2b7f319cccc" />Quinta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", 

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

Abriremos msfconsole, ejecutaremos el comando "searchsploit ssh enumerate" y seleccionaremos el exploit con dígito "1" con el comando "use 1", ejecutaremos options para ver lo que nos piden configurar, en este caso el RHOSTS que es la ip de la máquina víctima y el USER_FILE que le otorgaremos serán 2 diccionarios de usuarios básicos que es el top-usernames-shortlists.txt y el xato-net-10-million-usernames.txt, finalmente le daremos run, con el primero encontramos el usuario root y con el segundo el usuario lovely.

<img width="1521" height="784" alt="breakmyssh5" src="https://github.com/user-attachments/assets/15c6f238-2893-4d24-b643-deb143744054" />










## 🔑 ESCALADA DE PRIVILEGIOS

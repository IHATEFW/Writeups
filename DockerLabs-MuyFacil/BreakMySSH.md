Quinta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", 

# BREAKMYSSH

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1514" height="374" alt="breakmyssh1" src="https://github.com/user-attachments/assets/ee9f3a85-d972-4695-8e19-53ef796453e6" />

## 🔎 ENUMERACIÓN

Comenzamos enumerando los puertos abiertos con la herramienta nmap, esto nos permitirá dar el primer paso para comprometer y tantear el terreno de cara a comprometer la máquina, lanzaremos el siguiente el comando destacado en la evidencia, el cual nos arrojó que existe el puerto 22 abierto correspondiente al servicio SSH:

<img width="1503" height="785" alt="breakmyssh2" src="https://github.com/user-attachments/assets/c14fe5e1-aaaa-40f2-b8ff-fbaa640332d9" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

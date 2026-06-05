Esta máquina llamada Picadilly de la plataforma DockerLabs.es, es de dificultad fácil, la cual tiene expuesto el puerto 443 que nos permite subir un archivo malicioso, en este caso una reverse shell en php, del cual nos aprovechamos para ganar acceso a la máquina víctima . .

# PICADILLY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="851" height="589" alt="picadilly1" src="https://github.com/user-attachments/assets/ad8bfd69-1f16-4a2c-b489-cc1df7d5ec31" />

## 🔎 ENUMERACIÓN

El primer paso que realizaremos, ese la enumeración, en este caso utilizaremos la herramienta nmap para enumar todos los puertos que tenga abierta la máquina víctima, con el siguiente comando, una vez ejecutado, podemos visualizar que exísten los puertos 80 y 443 expuestos, correspondientes a los servicios HTTP y HTTPS.

<img width="881" height="631" alt="picadilly2" src="https://github.com/user-attachments/assets/39c68dae-5b26-495a-bb2e-db26d6e92282" />

Seguíremos enumerando con la herramienta nmap, pero esta vez indicandole a nmap que nos arroje la versión de dichos servicios, a su vez, tambien le indicaremos que nos ejecute un conjunto básico de scripts de reconocimiento, esto con el fin de encontrar más información que nos pueda servir, una vez ejecutado, podemos visualizar que 

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

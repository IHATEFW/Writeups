Esta máquina llamada Picadilly de la plataforma DockerLabs.es, es de dificultad fácil, la cual tiene expuesto el puerto 443 que nos permite subir un archivo malicioso, en este caso una reverse shell en php, del cual nos aprovechamos para ganar acceso a la máquina víctima . .

# PICADILLY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="851" height="589" alt="picadilly1" src="https://github.com/user-attachments/assets/ad8bfd69-1f16-4a2c-b489-cc1df7d5ec31" />

## 🔎 ENUMERACIÓN

El primer paso que realizaremos, ese la enumeración, en este caso utilizaremos la herramienta nmap para enumar todos los puertos que tenga abierta la máquina víctima, con el siguiente comando, una vez ejecutado, podemos visualizar que exísten los puertos 80 y 443 expuestos, correspondientes a los servicios HTTP y HTTPS.

<img width="881" height="631" alt="picadilly2" src="https://github.com/user-attachments/assets/39c68dae-5b26-495a-bb2e-db26d6e92282" />

Seguíremos enumerando con la herramienta nmap, pero esta vez indicandole a nmap que nos arroje la versión de dichos servicios, a su vez, tambien le indicaremos que nos ejecute un conjunto básico de scripts de reconocimiento, esto con el fin de encontrar más información que nos pueda servir, una vez ejecutado, podemos visualizar que exíste un achivo llamado backup.txt, tambien podemos visualizar que exíste una web llamada Picadilly detrás del puerto 443, vamos a revisar ambas.

<img width="844" height="647" alt="picadilly3" src="https://github.com/user-attachments/assets/e25c91b6-b4e6-46a6-b407-a2ff6c83824a" />

Ingresamos al archivo backup.txt y podemos visualizar que exíste un mensaje encriptado, aparente contraseña del usuario mateo, con un mensaje que dice "Para resolver este enigma, piensa en un antiguo emperador romando y su sencillo método de cambiar letras", de primera ya nos damos cuenta que hace referencia al cifrado César.

<img width="1143" height="504" alt="picadilly4" src="https://github.com/user-attachments/assets/ec00a7ba-ab70-4d0d-a836-79bb07ee9b24" />

Nos copiamos el mensaje encriptado, y en google buscamos alguna web que nos haga decoding en cifrado César de dicho mensaje:

<img width="1197" height="643" alt="picadilly5" src="https://github.com/user-attachments/assets/cfa23112-3466-4379-b091-2c0aa01ef422" />

Ya tenemos la contraseña, la guardamos para déspues, ahora procederemos a revisar la web detrás del puerto 443, vemos que la web nos deja subir un archivo, abrimos las herramientas del desarrollado o también llamado inspector para ver donde se guardaría dicho archivo, vemos un directorio /uploads.php.

<img width="1241" height="643" alt="picadilly6" src="https://github.com/user-attachments/assets/3fc8d1ac-b443-4bcc-8709-107c527646b1" />

<img width="819" height="654" alt="picadilly7" src="https://github.com/user-attachments/assets/c242dc85-63d5-42b9-9475-383b848293ef" />



## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

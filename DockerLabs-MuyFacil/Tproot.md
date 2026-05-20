Cuarta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina es casí identica a la llamada "FistHacking", ya que expone un servicio ftp de versión vsftpd 2.3.4 vulnerable a un backdoor, eso significa que abre una puerta trasera en el puerto 6200 del protocolo TCP, para así lograr ganar acceso a la máquina víctima, a continuación la resolveremos:

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1334" height="505" alt="tproot1" src="https://github.com/user-attachments/assets/e88f34db-e166-4137-a778-3365fab30139" />

## 🔎 ENUMERACIÓN

Se comienza enumerando los puertos abiertos con la herramienta nmap, arrojando el siguiente comando, esto nos permitirá ver los primeros vectores de entrada para poder comprometer esta máquina:

<img width="1516" height="785" alt="tproot2" src="https://github.com/user-attachments/assets/b7da4efe-504a-4c72-8677-b5f4eec7e293" />

Podemos visualizar que existen 2 puetos expuestos, el 21 y el 80, correspondientes a los servicios ftp y http, seguiremos enumerando de manera más exhaustiva con la herramienta nmap, ahora indicandole que nos arroje la versión de dichos servicios y que nos ejecute un conjunto básico de script de reconocimientos:

<img width="1516" height="788" alt="tproot3" src="https://github.com/user-attachments/assets/9dd42a15-b89b-48ee-87b3-2f8746e10bbd" />

Ya podemos ver las versiones que corren detrás de estos servicios, para el puerto 21 vemos la versión vsftpd 2.3.4 que mencionamos anteriormente que sabemos que es vulnerable y para el puerto 80 un Apache httpd 2.4.58, ahora procederemos a mirar la página web del puerto 80 a ver si encontramos algo:

<img width="1529" height="786" alt="tproot4" src="https://github.com/user-attachments/assets/741e7421-69d6-47e6-8b1e-6f1ef01fdc2d" />

Vemos que es la típica web por defecto de Apache2, exploramos el código fuente con CTRL + U, pero no vemos nada interesante, por lo tanto, en este punto sabemos que tendremos que explotar el puerto 21 


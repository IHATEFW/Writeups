
# FILE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="708" height="578" alt="file1" src="https://github.com/user-attachments/assets/38684070-00f6-48d5-a26c-32f7d0ff06c6" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, realizaremos un escaneo con la herramienta nmap, que nos arroje todos los puertos abiertos existentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen los puertos 21 y 80 expuestos, correspondientes a los servicios FTP y HTTP.

<img width="835" height="578" alt="file2" src="https://github.com/user-attachments/assets/b8c15b8c-395b-4ac7-bc64-a3b42f5b8f7d" />

Como ya sabemos los puertos abiertos, seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dichos servicios y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que se puede ingresar vía FTP con el usuario "Anonymous", dicho usuario no nos pedirá contraseña y en caso que lo haga podemos poner cualquiera y nos dejará ingresar igual.

<img width="851" height="633" alt="file3" src="https://github.com/user-attachments/assets/7e43a9e8-8283-4999-b5f2-0935808f63cb" />

Ingresamos por FTP y con el comando dir vemos que hay un archivo llamado anon.txt, lo descargaremos con el comando get a nuestra máquina atacante para revisarlo.

<img width="861" height="486" alt="file4" src="https://github.com/user-attachments/assets/47f4c7b3-65a1-42b6-bb64-897b062363ea" />

<img width="580" height="261" alt="file5" src="https://github.com/user-attachments/assets/80d3f34e-0f19-4219-aed0-7c786c1617ba" />

Vemos que se trata de una cadena cifrada, la decodearemos con la web hashes.com y nos muestra el nombre "justin", que puede ser un posible usuario válido.

<img width="864" height="520" alt="file6" src="https://github.com/user-attachments/assets/9a29352e-3a9f-4a5f-97f3-599392945644" />

Ya no tenemos nada más que hacer vía FTP, por lo tanto, miraremos la web que está por el puerto 80, vemos que está montada la tipica web Apache2 por defecto, le damos CTRL + U para ver el código fuente y encontramos una pista, indicando que existe un directorio raro.

<img width="779" height="638" alt="file7" src="https://github.com/user-attachments/assets/b8ee82a6-2541-4960-ac02-89dff24cb7b0" />

Procedemos a realizar fuerza bruta de directorios con la herramienta gobuster, encontrando un directorio llamado file_upload.php, en el cual se puede subir un archivo.

<img width="1334" height="638" alt="file8" src="https://github.com/user-attachments/assets/5ce89941-6add-4fb8-ac6c-89cc4dd13c99" />

<img width="759" height="284" alt="file9" src="https://github.com/user-attachments/assets/40a97124-08a0-4014-8a7c-13b41c0be073" />

Probamos subir la típica reverse shell de php que sacamos de revshells.com llamada PentestMonkey, pero no nos deja, indicando error al subirlo, eso quiere decirnos que la extensión .php no le gusta.-

<img width="759" height="284" alt="file10" src="https://github.com/user-attachments/assets/f9843929-81d9-435a-b734-8f7f86de785a" />

En este punto, abriremos Burpsuite para ver como se está tramitando la petición y poder realizar fuzzing de extensiones con un diccionario, para encontrar la extensión válida de php, activamos el foxyproxy, interceptamos la petición y la mandamos al Intruder. 

<img width="1187" height="582" alt="file11" src="https://github.com/user-attachments/assets/d99027b7-d2fb-4e0e-a082-c81e6d2d92e6" />

Ya en el intruder configuraremos el payload/carga útil seleccionando el siguiente diccionario de extensiones.

<img width="1187" height="582" alt="file12" src="https://github.com/user-attachments/assets/5c125fbb-148f-4ec1-9e23-bb1b1f05bc17" />

Estas son las extensiones que podrían resultar exitosas.

<img width="434" height="582" alt="file13" src="https://github.com/user-attachments/assets/16f4f023-e11b-43f3-add4-ff2303af32ee" />

Le damos "Start Attack" y encontramos que se subió solo el archivo pero con la extensión .phar, excelente trabajo.

<img width="1334" height="638" alt="file14" src="https://github.com/user-attachments/assets/25a3da60-6399-4fdc-abd1-37e8415e5157" />

<img width="681" height="582" alt="file15" src="https://github.com/user-attachments/assets/e0759171-85e2-410c-8ade-f285d1ac797c" />

Nos pondremos en escucha con netcat por el puerto 443 y daremos click en el archivo subido en /uploads, ¡Ganamos acceso a la máquina víctima!.

<img width="1037" height="380" alt="file16" src="https://github.com/user-attachments/assets/f364417b-e178-4ecf-9d97-81fdc0c678d6" />

<img width="746" height="603" alt="file17" src="https://github.com/user-attachments/assets/922789cb-8866-4e0f-be04-ae96c51460ac" />

<img width="1189" height="629" alt="file18" src="https://github.com/user-attachments/assets/5da702cc-bbca-42a4-98c1-45a89d271dde" />

<img width="984" height="240" alt="file19" src="https://github.com/user-attachments/assets/55d8f640-7152-423a-a0f5-c1d6dcd4d76c" />

<img width="682" height="284" alt="file20" src="https://github.com/user-attachments/assets/424da8b3-dc89-43fc-af6e-50d31f62ee8c" />

<img width="1139" height="561" alt="file21" src="https://github.com/user-attachments/assets/f1eabf22-99e5-4483-a2f0-280c8accc37d" />

<img width="636" height="406" alt="file22" src="https://github.com/user-attachments/assets/1b99a50d-076e-4452-b221-1a7437dd3034" />

<img width="1312" height="411" alt="file24" src="https://github.com/user-attachments/assets/03a44e59-24a6-43d2-a341-b4cfa0ee3f91" />

<img width="701" height="447" alt="file23" src="https://github.com/user-attachments/assets/79743572-618a-43a9-b8a9-e3c4711a1395" />

<img width="1312" height="411" alt="file24" src="https://github.com/user-attachments/assets/76448457-15d9-435f-88a2-1a8351a20423" />

<img width="724" height="502" alt="file25" src="https://github.com/user-attachments/assets/aaa1752c-4bdd-4a1b-a125-28757c80fb1c" />

<img width="1034" height="564" alt="file26" src="https://github.com/user-attachments/assets/b02732ae-c383-4b8b-93f0-22960ffae48e" />

<img width="969" height="281" alt="file27" src="https://github.com/user-attachments/assets/58ad4f17-a71e-4b62-a644-b6f3d3d51968" />

<img width="1184" height="609" alt="file28" src="https://github.com/user-attachments/assets/0efa30e1-d58a-4a91-b4ab-b11b6e6ee641" />

<img width="989" height="401" alt="file29" src="https://github.com/user-attachments/assets/87fb0090-c793-4458-b5a6-5f02e88de88b" />

<img width="996" height="473" alt="file30" src="https://github.com/user-attachments/assets/b3907f0a-a4a0-4d7e-90de-f3dfdd76be76" />

<img width="756" height="627" alt="file31" src="https://github.com/user-attachments/assets/1220986b-0ae5-4eea-a5fa-2f4f1090ac85" />

<img width="957" height="314" alt="file32" src="https://github.com/user-attachments/assets/3a5808df-1043-4745-8964-7c06f3fc43e6" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

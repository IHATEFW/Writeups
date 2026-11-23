La máquina Analyst de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual está enfocada más en el lado blueteam, donde hacemos el papel de analista de un SOC, en el cual nos entregan un ticket para resolver, sobre un atacante que consiguió subir un archivo sospechoso en el servidor web de la empresa, comenzamos revisando logs en base a 2 archivos que nos proporcionan, esto con la herramienta Wireshark, finalmente una vez se responden todas las preguntas se accede a visualizar las credenciales válidas del sistema, se pivota a root mediante permisos mal configurados a nivel de sudoers . .

# ANALYST

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1848" height="874" alt="ana1" src="https://github.com/user-attachments/assets/929a221c-ce36-476e-ae6b-d53c6fc9948d" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1819" height="656" alt="ana2" src="https://github.com/user-attachments/assets/e5e23442-600a-40ae-b69f-e5ff69b02304" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "El caso de Pinguinito / Lab Blue Team".

<img width="1808" height="786" alt="ana3" src="https://github.com/user-attachments/assets/3a7590c2-681e-4b30-9d27-8f2c2f0eea29" />

Revisaremos la página web detrás del puerto 80, una vez dentro, podemos visualizar un ticket de incidente, en el cual somos el analista asignado para resolverlo, ya que han detectado un archivo sospechoso dentro del servidor web.

<img width="1878" height="909" alt="ana4" src="https://github.com/user-attachments/assets/1314e7e3-f4fa-4a70-858c-607806d94289" />

Bajando un poco más, vemos 2 archivos que podemos descargar, un .pcap y un .json, indicando que abramos el .pcap con las herramientas Wireshark o tshark para revisar los logs, en este punto utilizaremos Wireshark.

<img width="1878" height="909" alt="ana5" src="https://github.com/user-attachments/assets/5d9f994e-1a5e-4d64-b160-cbb65ad932db" />

## 💣 EXPLOTACIÓN

Nos vamos a la terminal y abriremos Wireshark de la siguiente manera.

<img width="1191" height="225" alt="ana6" src="https://github.com/user-attachments/assets/60859b1f-7256-42d7-a315-330ff528254c" />

Abriremos el .pcap que nos facilitaron y vemos distintas peticiones, tanto HTTP, TCP, ARP, etc.

<img width="1882" height="911" alt="ana7" src="https://github.com/user-attachments/assets/8440b2a4-0744-46e1-9d2c-0816957ac367" />

Abriremos el caso y comenzaremos a responder las preguntas, la primera nos pregunta cual es la dirección ip real del atacante (la que permite subir el archivo sospechoso).

<img width="1882" height="911" alt="ana8" src="https://github.com/user-attachments/assets/1be2b28c-61d8-483a-9e98-003a37c20f68" />

Nos vamos a revisar las peticiones y encontramos un HTTP/1.1 200 OK con la ip del atacante, la examinamos y nos indica "Archivo subido correctamente".

<img width="1882" height="911" alt="ana9" src="https://github.com/user-attachments/assets/a454bd1c-c14a-485c-be40-be8d4f95501e" />

Verificamos y efectivamente es la respuesta correcta, vamos con la segunda, nos pregunta cual es el país de donde procede dicha ip.

<img width="1882" height="911" alt="ana10" src="https://github.com/user-attachments/assets/8ef8d549-c287-48b4-b5ec-b6fe4455f17e" />

En este punto, iremos a la terminal y leeremos el archivo .json que nos entregaron y nos aparece el país de donde proviene dicha ip, en este caso "Vietnam".

<img width="1882" height="911" alt="ana11" src="https://github.com/user-attachments/assets/f4bde241-c5a4-4b96-9fc7-032262984855" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la 3era pregunta, nos preguntan cual es el User-agent que usa el atacante en sus peticiones HTTP.

<img width="1882" height="911" alt="ana12" src="https://github.com/user-attachments/assets/916f3c7c-81bb-463d-b564-b9bb45bebf0e" />

Revisamos los logs y encontramos un HTTP GET /uploads/ HTTP/1.1 y encontramos el User-agent correspondiente del atacante, lo copiaremos para responder.

<img width="1882" height="911" alt="ana13" src="https://github.com/user-attachments/assets/e739722a-f6fd-4e96-864c-1267db634c75" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la 4ta pregunta, donde nos preguntan a que ruta (script) del servidor se envían las peticiones POST que suben el archivo.

<img width="1882" height="911" alt="ana14" src="https://github.com/user-attachments/assets/78732cf6-f4a0-4cf0-930e-a2ce0bdecbcc" />

Revisamos y encontramos una petición POST /reviews/upload.php HTTP/1.1, el cual es el script donde se envían las solicitudes POST para subir el archivo.

<img width="1882" height="911" alt="ana15" src="https://github.com/user-attachments/assets/06a4d601-f2a4-4bff-8ee3-bfe68befcb9e" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la 5ta pregunta, donde nos preguntan cual es la ruta real de subida de archivos del sitio.

<img width="1882" height="911" alt="ana16" src="https://github.com/user-attachments/assets/44f93bb1-3fc0-483b-a311-6f0ec3784a6f" />

Revisamos los logs y encontramos un HTTP GET /reviews/uploads/, dicha ruta es la real donde se suben los archivos, lo copiamos.

<img width="1882" height="911" alt="ana17" src="https://github.com/user-attachments/assets/fe780225-5de7-41ca-8e60-befa0813ac77" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la 6ta pregunta, donde nos pregunta como se llama el archivo que se consigue subirse.

<img width="1882" height="911" alt="ana18" src="https://github.com/user-attachments/assets/7741fd43-f0c6-4f5b-baa6-08a0af94d130" />

Encontramos una petición HTTP/1.1 200 OK donde se hace referencia al archivo /image.jpg.php, dicho archivo es el que se subió al servidor web, lo copiamos.

<img width="1882" height="911" alt="ana19" src="https://github.com/user-attachments/assets/e97c7d8c-ea58-4b32-92a2-27dbc659a5ff" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la 7ma pregunta, donde nos preguntan a que puerto se conecta/abre la conexión saliente.

<img width="1882" height="911" alt="ana20" src="https://github.com/user-attachments/assets/39add498-ae7d-4352-848b-56d33c739f63" />

Revisamos la siguiente petición donde se hace referencia al puerto 8080.

<img width="1882" height="911" alt="ana21" src="https://github.com/user-attachments/assets/d20e8822-2047-4f22-a0fe-4fc3b00711e9" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la pregunta 8va, donde nos preguntan a que fichero se intenta leer con el comando cat.

<img width="1882" height="911" alt="ana22" src="https://github.com/user-attachments/assets/90cdc864-80f4-46eb-91e6-29fee9bc0392" />

Revisamos y seguimos una petición en modo TCP stream donde se apertura un archivo con toda la información, el archivo que intenta leer el atacante es el /etc/passwd.

<img width="1600" height="900" alt="ana23" src="https://github.com/user-attachments/assets/0b8c4294-a31c-4c9f-85ce-2cf982ce4e08" />

Verificamos y efectivamente es la respuesta correcta, nos vamos a la última pregunta, donde nos preguntan cual es el nombre del usuario que el atacante ejecuta un script para resetearle la contraseña.

<img width="1600" height="900" alt="ana24" src="https://github.com/user-attachments/assets/bdd02752-a27d-4d3d-abb6-f73cbf108498" />

En el mismo archivo anterior nos indica que el usuario es "pinguinito".

<img width="1600" height="900" alt="ana25" src="https://github.com/user-attachments/assets/2d988167-c818-4c90-8f1a-b38616e92077" />

Verificamos y efectivamente es la respuesta correcta. 

<img width="1600" height="900" alt="ana26" src="https://github.com/user-attachments/assets/6b81543a-7d4b-4e13-8e15-25ae0f4954df" />

Ya finalizando toda la serie de pregunta, nos aparece la credencial filtrada para poder conectarnos por SSH, recordar que al principio cuando realizamos el escaneo con nmap también aparecía el puerto 22 abierto.

<img width="1600" height="900" alt="ana27" src="https://github.com/user-attachments/assets/16af9264-7890-4818-9c76-17a50588c244" />

## 🔑 ESCALADA DE PRIVILEGIOS

Nos logueamos por SSH con las credenciales que nos entregaron y ¡ganamos acceso a la máquina víctima", daremos el comando sudo -l para ver si podemos ejecutar binarios con permisos a nivel de sudoers y efectivamente podemos ejecutar cualquier binario como el usuario root, daremos sudo su y pivotamos de inmediato al usuario root, máquina hackeada . .

<img width="1600" height="900" alt="ana28" src="https://github.com/user-attachments/assets/1b04b64e-c03d-4671-8650-113b3529565e" />

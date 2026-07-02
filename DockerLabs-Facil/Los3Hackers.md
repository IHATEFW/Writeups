
# LOS 3 HACKERS

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="801" height="625" alt="los3hackers" src="https://github.com/user-attachments/assets/d04c9a28-1667-47fb-8379-18fe31fddad2" />

## 🔎 ENUMERACIÓN

Ya tenemos la ip de la máquina víctima, por lo tanto, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para descubrir los puertos abiertos/expuestos exístentes, con el fin de abusar de ellos, esto lo realizaremos de la siguiente manera, una vez ejecutado el escaneo, podemos visualizar que hay dos puertos abiertos, el 22 y el 80, relacionados a SSH y HTTP.

<img width="878" height="565" alt="los3hackers1" src="https://github.com/user-attachments/assets/6b80150b-6f67-4663-b204-7418432e7f88" />

Ya sabemos que los puertos 22 y 80 están abiertos, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dichos servicios y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que exíste una web llamada "Los 3 Hackers", la cual vamos a revisar.

<img width="881" height="637" alt="los3hackers2" src="https://github.com/user-attachments/assets/a7d12877-73fc-453f-a38e-62badb2ac12c" />

Una vez dentro de la web, podemos visualizar una temática relacionada a 3 hackers, un red hacker, otro blue hacker y el último black hacker.

<img width="1150" height="635" alt="los3hackers3" src="https://github.com/user-attachments/assets/9179039c-578e-4395-b282-6d01ac13e0cf" />

Bajando un poco más, vemos que se da la problemática que red hacker no recuerda su contraseña, y es el jugador quien deberá ayudarlo a encontrarla, aparentemente está dentro de algún dashboard, además, se visualiza un botón que se llama "AYUDA AL RED HACKER".-

<img width="1240" height="635" alt="los3hackers4" src="https://github.com/user-attachments/assets/b3c1f9c5-4f38-4c02-8f1a-6988fd9fa605" />

Nos redirecciona a un login de autenticación.

<img width="1240" height="635" alt="los3hackers5" src="https://github.com/user-attachments/assets/ca9e2fba-b70f-42db-b402-7931d959e162" />

Como no tenemos credenciales válidas, se procede a realizar el típico 'OR 1=1;-- - de SQLi, pero indica que el patrón no es válido, existe una blacklist bloqueando ciertos comandos por detrás.-

<img width="1240" height="635" alt="los3hackers6" src="https://github.com/user-attachments/assets/9c7374d6-f161-4bfd-b7a3-ce7c729e41b0" />

Se prueban distintos comandos y podemos visualizar que existe un rate limit en el login, ya que tras algunos intentos se bloquea por 30 segundos.

<img width="1240" height="635" alt="los3hackers7" src="https://github.com/user-attachments/assets/fc64233b-5191-43d8-b3fa-9adde562baf0" />

Finalmente nos damos cuenta que los comandos válidos son los siguientes:

<img width="529" height="331" alt="los3hackers8" src="https://github.com/user-attachments/assets/a2cf8345-0cc4-46ec-9f67-922a630c0f9b" />

¡Logramos acceder al dashboard!, dentro de aquel, podemos ver una "Flag", pero solo es una distracción, debido a que explica la vuln que se explotó.

<img width="1315" height="637" alt="los3hackers9" src="https://github.com/user-attachments/assets/e0475295-5d54-4683-bf85-f57754ef569a" />

Seguimos revisando y podemos visualizar un apartado que dice "Respaldos", accedemos a él y nos indica "Error 403" acceso denegado".

<img width="1225" height="637" alt="los3hackers10" src="https://github.com/user-attachments/assets/0f0c350d-3782-4e55-a144-59692ae896fb" />

Revisamos el código fuente con CTRL + U y vemos una pista que indica que efectivamente la credencial de red hacker no se encuentra dentro del dashboard.

<img width="1225" height="637" alt="los3hackers11" src="https://github.com/user-attachments/assets/36ed9c1c-2f6d-4cf2-a01c-39412a078f4c" />

Por lo tanto, en este punto, seguiremos enumerando pero esta vez con la herramienta GoBuster, logrando encontrar un archivo .zip, llamado wow.zip, el cual lo colamos en la URL despues de la /, y nos descarga automaticamente, pero OJO, se necesita estar logueado en el dashboard, por lo tanto, el jugador SIEMPRE debe explotar la SQLi antes de descargar el archivo.

<img width="1300" height="615" alt="los3hackers12" src="https://github.com/user-attachments/assets/d6876253-6c58-4163-be6a-de5e3bee55c2" />

<img width="1300" height="615" alt="los3hackers13" src="https://github.com/user-attachments/assets/6046fdf6-6e6b-47e2-b28d-32b22dba86f6" />

Una vez nos descargamos el comprimido, le damos unzip y visualizamos que ¡encontramos las credenciales de red hacker!

<img width="613" height="406" alt="los3hackers14" src="https://github.com/user-attachments/assets/f7e22b87-86de-4d77-a8e6-6831342ced64" />

Accedemos por SSH y ¡Ganamos acceso a la máquina!, procederemos en primera instancia a leer el archivo /etc/passwd, para visualizar si existen más usuarios en el sistema y efectivamente existen 2 usuario más "bluehacker y blackhacker", ahora cuadra la imágen de los hackers en la web detrás del puerto 80.

<img width="732" height="579" alt="los3hackers28" src="https://github.com/user-attachments/assets/00abec29-3cf1-483f-a950-4a9becac15fb" />

En el 

<img width="815" height="571" alt="los3hackers15" src="https://github.com/user-attachments/assets/14b5df00-0f03-4f65-a680-ecaaf445ac7c" />

<img width="659" height="293" alt="los3hackers16" src="https://github.com/user-attachments/assets/6253828d-e9c6-4f64-9879-3679f324bb28" />

<img width="562" height="215" alt="los3hackers17" src="https://github.com/user-attachments/assets/edaf979d-c924-4cda-bdd5-30bd72af5622" />

<img width="1227" height="588" alt="los3hackers18" src="https://github.com/user-attachments/assets/07eed70d-45b0-43ad-aecc-8ae7676ceba5" />

<img width="625" height="623" alt="los3hackers19" src="https://github.com/user-attachments/assets/0703bff5-f2ab-48bd-a932-2610ca2cd8ce" />

<img width="914" height="509" alt="los3hackers20" src="https://github.com/user-attachments/assets/e33d4811-d8a2-4715-b71a-ff4422584976" />

<img width="534" height="341" alt="los3hackers21" src="https://github.com/user-attachments/assets/e6cad113-dabc-459d-a190-f1c6eaf44e23" />

<img width="564" height="638" alt="los3hackers22" src="https://github.com/user-attachments/assets/05359e33-aec3-463e-80cd-fa5748c0a5f8" />

<img width="553" height="300" alt="los3hackers23" src="https://github.com/user-attachments/assets/153953f7-b6f0-4037-9910-25229d328efb" />

<img width="558" height="391" alt="los3hackers24" src="https://github.com/user-attachments/assets/5352d5df-f1b5-4234-9b30-e69929e12f45" />

<img width="587" height="272" alt="los3hackers25" src="https://github.com/user-attachments/assets/2eac39ff-da2d-4eea-9554-feb8137ccf50" />

<img width="1075" height="597" alt="los3hackers26" src="https://github.com/user-attachments/assets/92906a97-f43f-4ef0-8919-8fb0c26d4f89" />

<img width="642" height="633" alt="los3hackers27" src="https://github.com/user-attachments/assets/753b73e3-45f9-4079-96f2-f4d6a623c6ce" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

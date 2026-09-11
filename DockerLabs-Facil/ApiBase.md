La máquina ApiBase de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como interactuar con API's nos puede ayudar a enumerar rutas válidas e información sensible, como en este caso usuarios y sus credenciales, ya dentro de la máquina víctima logramos pivotar a root ya que su password estaba expuesta en un archivo dentro de /home. .

# APIBASE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="773" height="633" alt="api1" src="https://github.com/user-attachments/assets/050c4161-a730-42a1-a80a-1cef304633d1" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 5000, correspondientes a los servicios SSH y generalmente por el puerto 5000 corren servicios de Flask y Python.

<img width="1236" height="508" alt="api2" src="https://github.com/user-attachments/assets/87f7f2c6-9e33-4a1d-85f4-635c28afb92a" />

En este punto, seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión que corren detrás de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos darnos cuenta que efectivamente está corriendo el servicio Werkzeug de Python por el puerto 5000, vamos a mirarlo.

<img width="1052" height="633" alt="api3" src="https://github.com/user-attachments/assets/d16e06c7-7e23-4566-a472-1b4b7ab04124" />

Una vez lo miramos, nos damos cuenta que tendremos que lidiar con API's, indica que para añadir un usuario ocupemos /add y para consultar usuarios ocupemos /users

<img width="951" height="311" alt="api4" src="https://github.com/user-attachments/assets/a926130d-b1c3-48c0-ad7f-e6645d3d6953" />

En la terminal, lanzamos una petición con curl por el metodo GET a /users y nos devuelve "Invalid parameter"

<img width="900" height="279" alt="api5" src="https://github.com/user-attachments/assets/a3dcfdc0-2708-4c87-83cf-6476d1854fca" />

Probaremos con el metodo POST a /add de la siguiente manera y nos devuelve contenido interesante.

<img width="1009" height="622" alt="api6" src="https://github.com/user-attachments/assets/e2857f59-a34b-4616-84f1-fba047f9ecab" />

Si bajamos, podemos darnos cuenta que hace referencia a que deberemos ocupar el parametro "username" para enumerar a los usuarios.

<img width="1218" height="252" alt="api7" src="https://github.com/user-attachments/assets/f8c64671-d2ee-4b06-a1b1-f803607331d8" />

## 💣 EXPLOTACIÓN

Como ya tenemos /users y el parametro válido ?username, probaremos una SQLi, la típica 'OR 1=1 -- -, la cual nos devuelve un usuario válido y su password.

<img width="858" height="443" alt="api8" src="https://github.com/user-attachments/assets/429afaae-d7fa-4cf8-958b-90dce8ad07e4" />

Nos logueamos por SSH con las credenciales que obtuvimos y ¡Ganamos acceso a la máquina víctima!

<img width="954" height="237" alt="api9" src="https://github.com/user-attachments/assets/17759c4f-dbeb-4511-92cb-e9e2998ba28d" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina, nos vamos al directorio /home y leemos el archivo network.pcap y nos enumera la contraseña de root, finalmente pivotamos con éxito a root, máquina hackeada. .

<img width="806" height="593" alt="api10" src="https://github.com/user-attachments/assets/250a9ecf-3173-444f-9da5-ade92baad8e3" />

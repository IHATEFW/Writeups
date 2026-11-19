
# BEACH BAR

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1765" height="908" alt="beach1" src="https://github.com/user-attachments/assets/fbfd6582-3736-4bd9-8844-5483198183fa" />

<img width="1765" height="908" alt="beach2" src="https://github.com/user-attachments/assets/8717b6c3-7f18-4560-8ec9-8af272dce987" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente está encendida, a su vez le lanzamos un traceroute que nos indica que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64 que indica que nos enfrentamos a una máquina linux.

<img width="1765" height="908" alt="beach3" src="https://github.com/user-attachments/assets/9ae8c471-3c23-4c16-a108-13b5c35d13ae" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1777" height="745" alt="beach4" src="https://github.com/user-attachments/assets/25392c22-57db-44cb-8a84-b00bb5b1cc0e" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el http-title que corre en la web del puerto 80, que indica "Beach Bar // Sign in".

<img width="1794" height="856" alt="beach5" src="https://github.com/user-attachments/assets/fdc55f60-9707-46a3-9e8a-c6579b1c264a" />

Revisaremos la web del puerto 80, es un típico apartado de login de autenticación, probamos una inyección SQL pero no tenemos resultado éxitoso.

<img width="1794" height="856" alt="beach6" src="https://github.com/user-attachments/assets/c5ebc61b-e808-4aec-87f4-605468dc76c1" />

En este punto, como no tenemos credenciales válidas, revisaremos el código fuente de la web con CTRL + U para ver si existe alguna pista o alguna información que no veamos a simple vista, efectivamente encontramos un comentario que hace referencia a las credenciales dj:dj 

<img width="1794" height="856" alt="beach7" src="https://github.com/user-attachments/assets/79674527-f524-4485-bf24-a7f9d853c113" />

Logramos ingresar con dichas credenciales al dashboard, basicamente es como una plataforma de gestión de música para un bar o local nocturno, en el cual, se puede administrar la música que se reproducirá en el evento, en el apartado "Playlists" podemos exportar un archivo .yml y guardalo en nuestra máquina.

<img width="1794" height="856" alt="beach8" src="https://github.com/user-attachments/assets/c7d46e29-c524-461c-938d-8f7947649ce5" />

Lo guardamos y lo leemos por la terminal, indica una playlist, el género de la música, los artistas y los nombres de las canciones.

<img width="1735" height="764" alt="beach9" src="https://github.com/user-attachments/assets/a4358d2c-3f33-4eec-b848-8f3694212332" />

<img width="1762" height="910" alt="beach10" src="https://github.com/user-attachments/assets/f49194f9-62fd-4d5d-a608-c5aef7115fe4" />

<img width="1762" height="910" alt="beach11" src="https://github.com/user-attachments/assets/e3bb6c0c-c849-4177-81b3-444b894ad1eb" />

<img width="1762" height="910" alt="beach12" src="https://github.com/user-attachments/assets/b255b8ed-9394-4f0a-8e48-6875e6aacd33" />

<img width="1762" height="910" alt="beach13" src="https://github.com/user-attachments/assets/28db0b8c-5899-4898-8c42-a6fef8d1ef2a" />

<img width="1762" height="910" alt="beach14" src="https://github.com/user-attachments/assets/0ca5b3eb-b243-4611-96fa-14921627a413" />

<img width="1762" height="910" alt="beach15" src="https://github.com/user-attachments/assets/ba848123-9226-41c5-ae13-95672f0625b3" />

<img width="1762" height="910" alt="beach16" src="https://github.com/user-attachments/assets/8e401005-b1fd-46fe-aa5c-25e0bb75badb" />

<img width="1762" height="910" alt="beach17" src="https://github.com/user-attachments/assets/e373ebdb-5949-4e0a-b76b-83a3f4ed6530" />

<img width="1762" height="910" alt="beach18" src="https://github.com/user-attachments/assets/4d154823-53bb-46e2-8b2c-f6afcf250d46" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

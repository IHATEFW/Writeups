
# AUTOESCUELA

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1788" height="866" alt="autoescu1" src="https://github.com/user-attachments/assets/d79d8f61-7699-4db1-b03d-b17699ec0794" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 8080 y 9229, correspondientes a los servicios HTTP-PROXY y un servicio desconocido.

<img width="1788" height="866" alt="autoescu2" src="https://github.com/user-attachments/assets/8e664113-104b-4efb-9e35-01012bf4982f" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar que detrás del puerto 8080 corre un Node.js (Express middleware), además del http-title que hace referencia a: Autoescuela - Hackcar - Inicio. Por el puerto 9229 podemos ver que aparentemente espera conexiones WebSockets.

<img width="1788" height="866" alt="autoescu3" src="https://github.com/user-attachments/assets/44c30e87-e6a7-4103-9d14-81e354a09f84" />

Visitamos la página web del puerto 8080, es una academia para aprender a conducir, nada interesante.

<img width="1788" height="866" alt="autoescu4" src="https://github.com/user-attachments/assets/b8a375df-6d13-414a-b8ec-de56c5919b54" />

Por el puerto 9229 vemos que efectivamente espera peticiones tipo WebSockets.

<img width="1347" height="507" alt="autoescu5" src="https://github.com/user-attachments/assets/0ac917b7-bd70-40cb-a130-5db6f629c519" />

Volvemos a la terminal y probaremos con la herramienta curl enviar peticiones a los siguientes directorios, los cuales responden datos sensibles como un usuario válido del sistema /webuser y un archivo en /node_app/app.js

<img width="1891" height="875" alt="autoescu6" src="https://github.com/user-attachments/assets/9105c696-949f-4182-ad49-f78c64e283c9" />

Nos conectamos vía WebSocket de la siguiente manera, pero al enviarle distintas peticiones los puertos se cierran y tenemos que reiniciar la máquina.

<img width="1891" height="875" alt="autoescu7" src="https://github.com/user-attachments/assets/e9e266d6-3f9d-4cbb-8a16-b1cafc7d82dc" />

Probaremos conectarnos vía node inspect, otorgandole la ip de la máquina víctima y el puerto 9229, logramos una ejecución remota de comandos (RCE), por lo tanto, nos lanzaremos la típica reverse shell de bash para ganar acceso a la máquina.

<img width="1891" height="875" alt="autoescu8" src="https://github.com/user-attachments/assets/4ad3e49e-fefd-455f-95fa-9d1a9cc0c5d9" />

Sin antes ponernos en escucha con la herramienta netcat por el puerto 4444, lanzamos el comando anterior y ¡ganamos acceso a la máquina víctima!

<img width="1891" height="875" alt="autoescu9" src="https://github.com/user-attachments/assets/2327f685-da8f-48d7-a6df-04600c7624d0" />

Dentro de la máquina víctima, lo primero que haremos es leer el archivo /etc/passwd para ver si existen más usuarios válidos en el sistema para intentar pivotar hasta llegar a root, vemos que solamente existen los usuarios webuser y ubuntu.

<img width="1891" height="875" alt="autoescu10" src="https://github.com/user-attachments/assets/21a70841-1ec5-414d-8672-72bc0a48f042" />

En este punto encontramos en el home de webuser la flag user.txt, la probamos para subir a root pero sin éxito.

<img width="1891" height="875" alt="autoescu11" src="https://github.com/user-attachments/assets/456e3d7c-c80f-4de3-b3e0-d1d513459b67" />

Probaremos el comando ps -aux para ver que procesos se encuentran en ejecución y si existen procesos a nivel de crontab, vemos que root está ejecutando un servicio interno por el puerto 3000 por el propio localhost, el cual no es visible de momento, tendremos que realizar un portforwarding.

<img width="1891" height="875" alt="autoescu12" src="https://github.com/user-attachments/assets/216a1919-3980-4621-88c0-7fdc1b32e69a" />

Nos descargamos la herramienta chisel de Github, y levantamos un servidor por el puerto 80 con python3 para pasarlo a la máquina víctima, dicha herramienta chisel nos permitirá realizar el portforwarding.

<img width="1891" height="875" alt="autoescu13" src="https://github.com/user-attachments/assets/61e1859b-282c-43aa-934b-007c15cc5a45" />

Lo pasamos a la máquina víctima con el comando curl de la siguiente manera, siempre en /tmp.

<img width="1891" height="875" alt="autoescu14" src="https://github.com/user-attachments/assets/b9161abb-271f-41b5-87bd-9cdc5c706bd4" />

Levantaremos el tunel reverso de la siguienta manera por el puerto 1234, ya se encuentra en listening.

<img width="1891" height="875" alt="autoescu15" src="https://github.com/user-attachments/assets/a69c1a74-98f4-4ac4-8b4b-4099a9cc7bb4" />

Y el puerto 3001 de la máquina víctima lo convertiremos en el puerto 3000, y está listo.

<img width="1891" height="875" alt="autoescu16" src="https://github.com/user-attachments/assets/c6266418-7f7f-4738-afeb-e1561cb30e64" />

Revisamos el panel interno que no teníamos acceso y hace referencia a una versión 15.0.0-rc.1

<img width="1891" height="875" alt="autoescu17" src="https://github.com/user-attachments/assets/6cbaa40a-9742-49a6-ac46-21f95ee83ab5" />

Dicha versión es vulnerable a un React2shell, cuyo exploit lo encontramos en el siguiente repositorio de Github.

<img width="1891" height="875" alt="autoescu18" src="https://github.com/user-attachments/assets/ad484d6e-31aa-479f-90b4-d430bf9419d6" />

Lo traemos a nuestra máquina atacante con el comando wget de la siguiente manera y nuevamente levantaremos un servidor con python3 por el puerto 80 para pasarlo a la máquina víctima.

<img width="1891" height="875" alt="autoescu19" src="https://github.com/user-attachments/assets/1421a84b-1aac-40bd-a73d-8134993acf0e" />

En la máquina víctima lo traeremos con el comando curl de la siguiente manera.

<img width="1891" height="875" alt="autoescu20" src="https://github.com/user-attachments/assets/754a6fcd-b714-4642-a579-f605f7106a22" />

Según las indicaciones del exploit, lo lanzamos de la siguiente manera adjuntándole el comando whoami para ver si somos root.

<img width="1891" height="875" alt="autoescu21" src="https://github.com/user-attachments/assets/4c46052b-64cf-45a1-8480-5d8c8714a573" />

Decodificamos la cadena en base64 y efectivamente el comando se interpreta como el usuario root.

<img width="1367" height="465" alt="autoescu22" src="https://github.com/user-attachments/assets/aa0e86ce-49c1-4974-a623-a47c77d512a3" />

En la máquina atacante nos creamos un archivo llamado reverse.sh que contiene el típico oneliner de reverse shell de bash.

<img width="1327" height="372" alt="autoescu23" src="https://github.com/user-attachments/assets/0b6f2d5c-ebab-4967-90f1-3e3ece20c29b" />

Lo pasamos nuevamente a la máquina víctima con el servidor de python3 por el puerto 80 arriba.

<img width="1327" height="372" alt="autoescu24" src="https://github.com/user-attachments/assets/46e651ba-8b7b-45c8-a966-22b1143e63ce" />

En la máquina víctima lo traemos con el comando curl, sin antes habernos puesto por escucha nuevamente con netcat por el puerto 443

<img width="1546" height="515" alt="autoescu25" src="https://github.com/user-attachments/assets/20da43fe-9636-4f08-9129-822a4ea41e11" />

¡Ganamos acceso a la máquina víctima! nuevamente pero esta vez como root, máquina hackeada . .

<img width="1843" height="867" alt="autoescu26" src="https://github.com/user-attachments/assets/7be5c14e-1203-4e21-b47a-89ea4dcb0a2e" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

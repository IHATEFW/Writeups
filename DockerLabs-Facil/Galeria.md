# GALERIA

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="728" height="601" alt="galeria1" src="https://github.com/user-attachments/assets/2d3e0ba4-96fd-49b7-9690-1cf8f98645dc" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que solamente exíste el puerto 80 expuesto, correspondiente al servicio HTTP.

<img width="777" height="517" alt="galeria2" src="https://github.com/user-attachments/assets/d50b1e8c-2449-4cf4-b07b-be472e1641f0" />

Ya sabemos que está el puerto 80 abierto, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que exíste una web llamada "Gallery", la cual vamos a revisar.

<img width="808" height="635" alt="galeria3" src="https://github.com/user-attachments/assets/b81951e6-b1c1-4dfc-b560-ce79178f4ed8" />

<img width="1321" height="619" alt="galeria4" src="https://github.com/user-attachments/assets/dc77d7a6-1d1d-49b8-9b12-e005a23ad854" />

Vemos que la web está ambientada en civilizaciones antiguas, en una especie de galeria, daremos CTRL + U para exáminar el código fuente y vemos que en la URL de las imágenes exíste un directorio llamado /uploads, accederemos a él y vemos que hay un archivo llamado handler.php, el cual nos permite realizar una subida de archivos.

<img width="752" height="640" alt="galeria5" src="https://github.com/user-attachments/assets/f815a451-a69a-458e-8b3d-6ab88103f257" />

<img width="546" height="389" alt="galeria6" src="https://github.com/user-attachments/assets/dbf7e7e9-f87b-4635-abf0-513d8cb68e73" />

<img width="546" height="389" alt="galeria7" src="https://github.com/user-attachments/assets/55b3ee3f-4847-4359-820e-fa8bcb242fec" />

## 💣 EXPLOTACIÓN

Lo primero que se me ocurre es subir una reverse shell en php, esta la sacaremos de la web revshells.com, es la que se llama PentestMonkey, la configuraremos con nuestra ip de la máquina atacante y con cualquier puerto que queramos, nos la descargamos en un archivo .php y la subimos al campo de subida, nos ponemos en escucha con netcat y le daremos click al archivo que hemos subido (recordar que se subirá al directorio /uploads/image, y listo, ¡ya ganamos acceso a la máquina víctima!

<img width="1217" height="615" alt="galeria8" src="https://github.com/user-attachments/assets/f93990df-73b0-4176-a450-04f15c9a65e6" />

<img width="786" height="606" alt="galeria9" src="https://github.com/user-attachments/assets/cc5a8922-bcac-43c4-ab02-5cc4a521cd4b" />

<img width="521" height="236" alt="galeria10" src="https://github.com/user-attachments/assets/de2c3c64-c0bd-4b9a-a409-d128954f6aac" />

<img width="1029" height="381" alt="galeria11" src="https://github.com/user-attachments/assets/8e8b38fc-4422-4540-8719-d3c15fb53dc7" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```

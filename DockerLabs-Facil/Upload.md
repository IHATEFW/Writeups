
# UPLOAD

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="550" height="344" alt="upload1" src="https://github.com/user-attachments/assets/3372695c-cf09-4e2a-8986-523c5420314f" />

## 🔎 ENUMERACIÓN

Ya tenemos la ip de la máquina víctima, por lo tanto, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para descubrir los puertos abiertos/expuestos exístentes, con el fin de abusar de ellos, esto lo realizaremos de la siguiente manera, una vez ejecutado el escaneo, podemos visualizar existe el puerto 80 abierto, relacionado a HTTP.

<img width="779" height="534" alt="upload2" src="https://github.com/user-attachments/assets/ab9d3dd5-bd4f-4393-adb2-d2ac17febaf6" />

Ya sabemos que el puerto 80 está abierto, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que exíste una web "Upload here your file", la procederemos a revisar.

<img width="829" height="641" alt="upload3" src="https://github.com/user-attachments/assets/356e5c52-6fbb-4732-9758-6b1edb21bfe5" />

Una vez dentro, vemos que se puede subir un archivo.

<img width="1220" height="641" alt="upload4" src="https://github.com/user-attachments/assets/32f6817b-2e08-4487-99ca-72eff6181cfa" />

Probamos subiendo una reverse shell .php, la cual la configuramos en la web revshells.com, con nuestra ip atacante y puerto que queramos ponernos en escucha, la subimos y vemos que se sube exitosamente.

<img width="548" height="276" alt="upload5" src="https://github.com/user-attachments/assets/73c9d6da-fdd1-4dc5-ad96-399f28364029" />

En este punto, procederemos a realizar un ataque de fuerza bruta con GoBuster para lograr identificar el directorio donde se almacena dicho archivo que subidos, y encontramos /uploads.

<img width="876" height="638" alt="upload6" src="https://github.com/user-attachments/assets/c289480b-15d9-48db-a6a3-0691350cee33" />

Ingresamos y está la reverse shell que subimos.

<img width="876" height="638" alt="upload7" src="https://github.com/user-attachments/assets/8c0e8e2d-b651-47dd-8358-756d0a086a8d" />

Nos ponemos en escucha con netcat por el puerto 443, ingresamos al archiv y ¡Ganamos acceso a la máquina víctima!

<img width="1029" height="409" alt="upload8" src="https://github.com/user-attachments/assets/b4fe741e-dd2a-486c-9210-845c1d24b96e" />

<img width="1028" height="275" alt="upload9" src="https://github.com/user-attachments/assets/8972bac3-03c9-45bb-95b0-b3c5932be8c7" />

<img width="1130" height="609" alt="upload10" src="https://github.com/user-attachments/assets/82235ee8-4f02-4514-bcbb-2cca45e64290" />

<img width="518" height="347" alt="upload11" src="https://github.com/user-attachments/assets/9511bf8f-90e2-43dd-ac65-514fbe7f45d4" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

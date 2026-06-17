La máquina Walking Dead de la plataforma DockerLabs.es es una máquina de dificultad Fácil, que nos enseña como útilizar fuzzing con la herramienta wfuzz, ya que encontramos un directorio /.shell.php, encontrando el parámetro perfecto para pasar de un LFI a un RCE, luego abusamos de un binario SUID para pivotar a root. .

# WALKING DEAD

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="758" height="579" alt="walking1" src="https://github.com/user-attachments/assets/5083ed44-4860-4ef1-88ea-d67ba1bce3df" />

## 🔎 ENUMERACIÓN

Para comprometer esta máquina, primero realizaremos comenzaremos la enumeración realizando un escaneo de puertos con la herramienta nmap, esto con el fin de poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado podemos visualizar que tenemos dos puertos abiertos, el 22 y el 80, correspondientes a los servicios SSH y HTTP.

<img width="821" height="514" alt="walking2" src="https://github.com/user-attachments/assets/db818163-fae0-4f29-8777-d9e84bc4846f" />

Una vez ejecutado el primer escaneo y ya teniendo los puertos abiertos en mano, continuaremos realizando otro escaneo con la herramienta nmap, pero esta vez más exhaustivo, indicandole a nmap que nos encuentre la versión de dichos servicios, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar el nomnbre de la web que corre detrás de puerto 80, se llama "The Walking Dead - CTF", procederemos a revisarla:

<img width="923" height="631" alt="walking3" src="https://github.com/user-attachments/assets/f9aba068-3f45-4f92-bf40-4b7435e2c626" />

<img width="1220" height="608" alt="walking4" src="https://github.com/user-attachments/assets/3ab84004-4344-4a46-9d8f-0fe70ba05c26" />

Vemos que no dice "Sobrevive si puedes", no encontramos nada interesante, por lo tanto, procederemos a revisar el código fuente con CTRL + U, al final, vemos un directorio llamado /hidden/.shell.php

<img width="1241" height="382" alt="walking5" src="https://github.com/user-attachments/assets/37c61c72-7dc5-4e79-af2e-d622140edcf6" />

## 💣 EXPLOTACIÓN

Por lo cual, en este punto procederemos a realizar fuzzing parámetros para concatenar detrás del signo ?, algo como php?FUZZ=id, esto con el fin de lograr un inyectar comandos a nivel de sistema, esto con el siguiente comando, una vez ejecutado, podemos visualizar que nos encontró el parámetro "cmd".

<img width="1244" height="573" alt="walking6" src="https://github.com/user-attachments/assets/cb9c0240-ab22-4b6a-bdbd-865f719f9b7f" />

Revisamos y lo inyectamos en la url, de la mano con el comando "id", y efectivamente podemos ejecutar comandos a nivel de sistema, nos devuelve el usuario que somos dentro de la máquina (www-data).

<img width="775" height="215" alt="walking7" src="https://github.com/user-attachments/assets/1b97cbcf-4804-4e64-8a70-aefb5ccb07d4" />

Tambien podemos leer el archivo /etc/passwd, dandonos cuenta que existen 2 usuarios más antes que root (rick y negan).

<img width="1355" height="435" alt="walking8" src="https://github.com/user-attachments/assets/f80be05a-5361-4053-af1e-7bf07bc03f04" />

Nos preparamos la típica reverse shell de bash, con el siguiente comando:

<img width="928" height="210" alt="walking9" src="https://github.com/user-attachments/assets/1235bd98-a973-4f86-be73-23ccafae63d8" />

Nos ponemos en escucha por la terminal con la herramienta netcat por el puerto que queramos, ejecutamos el comando en la web y ¡Ganamos acceso a la máquina víctima!.

<img width="700" height="347" alt="walking10" src="https://github.com/user-attachments/assets/595ddf37-6027-4d07-b5b8-26fa4286c606" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ejecutaremos los siguientes comandos para tener una tty estable, esto se llama tratamiento de la tty y se realiza con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```

Una vez dentro de la máquina víctima, daremos el siguiente comando para ver si tenemos permisos para ejecutar binarios SUID:

```bash
find / -perm -4000 2>/dev/null
```

<img width="578" height="398" alt="walking11" src="https://github.com/user-attachments/assets/350246ad-8ac3-4db1-98eb-68d49cb352a1" />

Y efectivamente podemos ejecutar el binario python3.8 como root.

<img width="1193" height="584" alt="walking12" src="https://github.com/user-attachments/assets/e1cbedba-cea7-4dcb-a265-fe3546a8c9b5" />

<img width="784" height="458" alt="walking13" src="https://github.com/user-attachments/assets/172d0dca-ab57-4d1b-a989-c7a93cb1ab33" />


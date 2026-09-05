La máquina ChocolateLovers de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como explotar un CMS llamado Nibbleblog, el cual permite gestionar blogs con pocos recursos, esto lo hicimos subiendo una reverse shell dentro del plugin "My Image", ya dentro de la máquina víctima logramos pivotar a root abusando de permisos a nivel de sudoers y alterando un script que se ejecuta como root cada 5 segundos . .

# CHOCOLATELOVERS

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1011" height="449" alt="choco1" src="https://github.com/user-attachments/assets/c4c3602d-9ddb-4755-9bbd-56a7841df130" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que solo existe el puerto 80 abierto, correspondiente al servicio HTTP.

<img width="1228" height="625" alt="choco2" src="https://github.com/user-attachments/assets/235ace51-99de-4ea6-acbc-e9fdb0fb3e56" />

Seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio HTTP que está corriendo, esto de la siguiente manera, una vez ejecutado, podemos darnos cuenta que es la típica página web por defecto de Apache2, sin más novedades.

<img width="1228" height="625" alt="choco3" src="https://github.com/user-attachments/assets/66c89f85-bbea-440b-9c81-4c60392621c0" />

Vamos a revisarla y efectivamente es la web por defecto.

<img width="1228" height="625" alt="choco4" src="https://github.com/user-attachments/assets/6840e604-8577-4c7b-bf55-49440ab1f094" />

Revisaremos el código fuente con CTRL + U para ver si existe alguna pista que no estemos viendo y efectivamente se hace referencia a un CMS llamado Nibbleblog, en el directorio /nibbleblog.

<img width="1228" height="625" alt="choco5" src="https://github.com/user-attachments/assets/1072af0a-e9cf-49f9-8a6f-626ab9ba3e37" />

Lo revisamos y encontramos la siguiente web, básicamente este CMS se encarga de gestionar un blog sin base de datos como MySQL.

<img width="1228" height="625" alt="choco6" src="https://github.com/user-attachments/assets/2f1986b5-0aba-4ed4-8c3e-f7cff77f51db" />

Nuevamente abrimos el código fuente con CTRL + U y vemos que existe un /nibbleblog/feed.php

<img width="1228" height="625" alt="choco7" src="https://github.com/user-attachments/assets/3a5a450b-9a9c-4cb9-9597-e579e1fea801" />

Nos metemos y hace referencia a /nibbleblog/admin.php

<img width="1228" height="625" alt="choco8" src="https://github.com/user-attachments/assets/d7c924f6-4863-40ef-96e6-887b06fe1954" />

Lo revisamos y nos encontramos a una panel de login de dicho CMS, probaremos las siguientes credenciales admin:admin

<img width="1228" height="625" alt="choco9" src="https://github.com/user-attachments/assets/5d9e3ce7-e095-40fe-894f-549a91e73d5b" />

Logramos acceder al dashboard.

<img width="1228" height="625" alt="choco10" src="https://github.com/user-attachments/assets/a4106fb7-918b-4908-88d1-388da606578c" />

En el apartado de "Settings" scrolleamos hasta el final y vemos la versión de dicho Nibbleblog.

<img width="1228" height="625" alt="choco11" src="https://github.com/user-attachments/assets/c4aab4cf-f16f-45c0-85b7-ab79473b6cc8" />

Buscamos en la web algun exploit que tenga Nibbleblog y nos hace referencia a que la versión 4.0.3 es vulnerable a un RCE vía subida de archivos en el plugin "My Image".

<img width="1228" height="633" alt="choco12" src="https://github.com/user-attachments/assets/c87e9641-aa7f-421d-970a-e51f52199372" />

## 💣 EXPLOTACIÓN

Lo buscamos y efectivamente tiene un campo de subida de archivos, se nos ocurre subir una reverse shell en .php

<img width="1228" height="633" alt="choco13" src="https://github.com/user-attachments/assets/c39133f3-9b00-401b-8f4c-a834b24c1bb2" />

En otra web, logramos identificar la URL de donde ejecutarlo.

<img width="1228" height="633" alt="choco14" src="https://github.com/user-attachments/assets/fe6a438c-655e-4b5d-8d58-7a82497c5d66" />

Nos levantamos un listening con la herramienta netcat por el puerto 443.

<img width="612" height="232" alt="choco15" src="https://github.com/user-attachments/assets/d4521c8a-1191-487c-b010-37a11140e624" />

Lanzamos la reverse shell y ¡Ganamos acceso a la máquina víctima!

una vez dentro de la máquina víctima, procedemos a realizar el tratamiento de la tty con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 26 columns 148
```
<img width="1188" height="366" alt="choco16" src="https://github.com/user-attachments/assets/fca2c18e-2bfb-46cd-bb74-bb6d3813911e" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya con la tty más estable, procederemos a leer el archivo /etc/passwd para ver si existen más usuarios dentro de la máquina a los cuales tendremos que pivotar y nos encontramos con el usuario chocolate.

<img width="750" height="572" alt="choco17" src="https://github.com/user-attachments/assets/cf77fc66-f63f-4160-b111-559ee977179a" />

Damos un sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algun binario con permisos de otros usuarios y efectivamente podemos ejecutar php como el usuario chocolate, lanzamos el siguiente comando y logramos pivotar a chocolate.

<img width="1087" height="408" alt="choco18" src="https://github.com/user-attachments/assets/bc09cb6a-9cec-45b7-a9a4-3d142976f69d" />

Seguimos revisando y podemos ver que en /opt existe un script.php del cual somos dueño y que se ejecuta cada 5 segundos como el usuario root, esto lo corroboramos con un ps -aux, para eso comprobamos creando un directorio en /tmp llamado proof con el comando id y vemos que efectivamente el script.php se ejecuta como root, procedemos a modificar el binario /bin/bash para que se vuelva SUID y lanzamos una bash privilegiada con bash -p, finalmente somos root, máquina hackeada . .

<img width="854" height="566" alt="choco19" src="https://github.com/user-attachments/assets/8fe4a961-4c34-4c56-9d8b-4dae916cf36d" />

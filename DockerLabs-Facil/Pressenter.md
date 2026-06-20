
# PRESSENTER

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="747" height="616" alt="pressenter1" src="https://github.com/user-attachments/assets/b0691f01-081f-46f5-a8eb-74dc1a2d7ff7" />

## 🔎 ENUMERACIÓN

Como primero paso, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para lograr identificar los puertos abiertos/expuestos de la máquina víctima, esto con el siguiente comando, una vez ejecutado, podemos visualizar que existe el puerto 80 abierto, relacionado al servicio HTTP.

<img width="768" height="516" alt="pressenter2" src="https://github.com/user-attachments/assets/2987e0f4-60ad-41d2-b360-9cc00f8abb01" />

Una vez ya tenemos identificado el único puerto abierto, seguiremos enumerando con nmap, pero esta vez con un escane más exhaustivo, indicandole a nmap que nos encuentra la versión de dicho servicio, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, esto con el siguiente comando, una vez ejecutado, podemos ver que el titulo de la web es "Pressenter CTF", vamos a revisar dicha web.

<img width="807" height="636" alt="pressenter3" src="https://github.com/user-attachments/assets/3452d6e8-e000-4a67-8f5c-dd43162b6bda" />

<img width="1243" height="636" alt="pressenter4" src="https://github.com/user-attachments/assets/2bd43a0c-3d45-4d2a-a71c-a865e2666bcf" />

Ya dentro de la web, podemos visualizar que esteticamente no es muy amigable, por lo tanto, vamos a revisar el código fuente con CTRL + U y nos damos cuenta que hace referencia a un dominio "pressenter.hl", por lo tanto, lo vamos agregar a nuestro archivo /etc/hosts para ver si la web responde distinto.

<img width="1229" height="279" alt="pressenter5" src="https://github.com/user-attachments/assets/d6416418-2889-4685-898d-37b1015e2636" />

Una vez ya agregado el dominio con la ip a dicho archivo, procederemos a recargar la web con F5.

<img width="786" height="330" alt="pressenter6" src="https://github.com/user-attachments/assets/ff960d10-0e5d-462c-81ee-7285acdd1a61" />

Y vemos que efectivamente la web se ve mucho más bonita y nos muestra la información distinta, podemos visualizar que estamos ante la típica web desarrollada con el CMS WordPress.

<img width="1223" height="628" alt="pressenter7" src="https://github.com/user-attachments/assets/3155b533-59af-4367-90bb-7440e0d08b76" />

Damos nuevamente CTRL + U, y filtramos por la palabra 'generator', donde nos muestra la versión de dicho WordPress que es la 6.6.1.

<img width="1115" height="573" alt="pressenter8" src="https://github.com/user-attachments/assets/d6e65442-806d-4da8-9317-fbd2479f328a" />

En este punto, ya sabemos que nos estamos enfrentando a dicho CMS, por lo tanto, existe una herramienta que viene preinstalada en parrot o kali, que se llama wpscan, la cual nos permite enumerar dicho WordPress, nos enumera usuarios, contraseñas válidas, plugins, la versión, etc, vamos a ocuparla con el siguiente comando:

<img width="752" height="631" alt="pressenter9" src="https://github.com/user-attachments/assets/30417a55-ebce-4919-b2ac-bab28ee7b9cb" />

Una vez ejecutado, podemos visualizar que nos enumeró usuarios válidos, como "pressi" y "hacker".

<img width="752" height="631" alt="pressenter10" src="https://github.com/user-attachments/assets/d0ab3cbf-911b-4146-9660-d9c30a2f4de8" />

Ya teniendo en nuestras manos usuarios válidos, vamos a seguir enumerando con la herramienta wpscan, pero esta vez, le cambiaremos los parametros para otorgarle el rockyou.txt y que nos encuentre la contraseña de dichos usuarios, esto con el siguiente comando:

<img width="752" height="631" alt="pressenter11" src="https://github.com/user-attachments/assets/5e0b52f4-b169-4b2e-917d-32b4e0c1c91d" />

¡Y efectivamente nos encuentra la contraseña del usuario pressi!, vamos a probarla en el login que existe en la dirección /wp-login.php, que es un panel de autenticación para ingresar al dashboard.

<img width="752" height="631" alt="pressenter12" src="https://github.com/user-attachments/assets/b3fe5989-2ec0-488b-a636-b0c3aec7f6ee" />

Es la contraseña correcta y ganamos acceso al dashboard de WordPress.

<img width="1346" height="631" alt="pressenter13" src="https://github.com/user-attachments/assets/fbdf8d09-ae15-4d76-a2c0-7cb210b752dc" />

Nos dirigimos al apartado donde dice "Plugins" y vemos que se encuentra el plugin "Hello Dolly", que actualmente no está activado.

<img width="1346" height="631" alt="pressenter14" src="https://github.com/user-attachments/assets/a5a41b62-01b6-4189-8c7e-36b21c63e5d4" />

Ahora nos vamos al apartado donde dice "Herramienta" y accedemos a editar los plugins instalados, dicho plugin mencionado anteriormente tiene un archivo llamado hello.php, que es básicamente puro código php.

<img width="1346" height="631" alt="pressenter15" src="https://github.com/user-attachments/assets/c72d0af0-f5ae-4d9d-a806-0a8cb526f33b" />

Se nos ocurre dirigirnos a la web revshells.com, donde filtraremos por "php" y la última reverse shell vamos a escoger. 

<img width="1235" height="631" alt="pressenter16" src="https://github.com/user-attachments/assets/7df76b75-b061-4f20-8a73-bac2ea37de79" />

La copiamos y la pegamos de la siguiente manera en el archivo, lo guardamos.

<img width="784" height="330" alt="pressenter17" src="https://github.com/user-attachments/assets/d3f2c597-6e5a-47cf-98bc-05f2522b2e5c" />

Nos ponemos en escucha en una terminal con la herramienta netcat por el puerto 443, volvemos al dashboard y activamos el plugin, y ¡Ganamos acceso a la máquina víctima! 🔥

<img width="785" height="250" alt="pressenter18" src="https://github.com/user-attachments/assets/818ae73b-0777-4bba-a9da-c2ffc65343b6" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```
Ya con la tty estable, leeremos el archivo llamado wp-config.php.

<img width="616" height="539" alt="pressenter19" src="https://github.com/user-attachments/assets/33a10ba8-e29a-44f6-b6c7-5da3a795685b" />

En el cual encontramos el usuario admin con su contraseña de mysql.

<img width="616" height="539" alt="pressenter20" src="https://github.com/user-attachments/assets/e93d30e5-dd50-4996-b191-aefe3c12ed1d" />

Ingresaremos con el siguiente comando:

```bash
mysql -u admin -p
```

Y listaremos las bases de datos con el comando:

```bash
SHOW DATABASES;
```

<img width="633" height="388" alt="pressenter21" src="https://github.com/user-attachments/assets/c5c8f5d1-1bc2-4485-9275-06924f5574a3" />

Encontramos una base de datos llamada "wordpress", accederemos a ella y haremos que nos muestre las tablas.

<img width="612" height="393" alt="pressenter22" src="https://github.com/user-attachments/assets/fa2e0e54-a6ef-49f4-8b76-1bd6b680c150" />

Seleccionaremos la tabla que nos parezca más importante y ¡encontramos la contraseña del usuario "enter"! 🔥

<img width="526" height="137" alt="pressenter23" src="https://github.com/user-attachments/assets/08095f11-9d8c-416d-9ced-787429aa97e7" />

Subiremos al usuario enter con esa contraseña y daremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algun binario, vemos que podemos ejecutar /usr/bin/cat y /usr/bin/whoami

<img width="1026" height="182" alt="pressenter24" src="https://github.com/user-attachments/assets/572ba525-6a36-4d02-b212-cafe9b827bf5" />

Luego de un rato intentando subir privilegios, nos damos cuenta que dichos permisos sudoers fueron solo utilizados para distraernos ya que la password del usuario root es la misma que la del usuario enter, aquí se utilizó reutilización de contraseñas, así que de una nos metemos como usuario root, ¡máquina hackeada! 🔥. .

<img width="519" height="496" alt="pressenter25" src="https://github.com/user-attachments/assets/eb9fa2c3-5733-4d44-b6f9-dd19673d9a27" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS


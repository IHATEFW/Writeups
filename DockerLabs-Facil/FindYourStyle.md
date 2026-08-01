
# FINDYOURSTYLE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="892" height="630" alt="find1" src="https://github.com/user-attachments/assets/5f5510f5-1e0d-4ef5-b297-40682035aaa9" />

## 🔎 ENUMERACIÓN

Cuando ya tengamos la ip de la máquina víctima, se procederá a realizar un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos existentes abiertos/expuestos que tenga la máquina, esto con el siguiente comando, una vez ejecutado, podemos visualizar que solamente tenemos el puerto 80 abierto, relacionado al servicio HTTP.

<img width="1208" height="593" alt="find2" src="https://github.com/user-attachments/assets/612087d7-c140-42bd-81f0-b118491a628c" />

Ya cuando tengamos los puertos abiertos identificados, seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que al parecer la web que hay por detrás del puerto 80 expone un gestor de contenido CMS llamado Drupal, en su versión 8, además, existe el archivo robots.txt donde se exponen más directorios válidos.

<img width="937" height="628" alt="find3" src="https://github.com/user-attachments/assets/154562b8-5d4e-4753-ab96-44820d21e123" />

En este punto, vamos a revisar la web Drupal 8, accedemos y nos encontramos con esta web "Find your own Style".

<img width="1317" height="628" alt="find4" src="https://github.com/user-attachments/assets/84423b3c-5273-4a51-a8ee-573158aee1f4" />

Presionamos el botón "Login in" e intentamos loguearnos con una SQLi (Inyección SQL), ya que no tenemos credenciales válidas, pero no podemos.

<img width="985" height="621" alt="find5" src="https://github.com/user-attachments/assets/1f4474e6-119a-4eac-8844-47288e9854d7" />

Ahora iremos a la terminal, para hacer utilidad de la base de datos de exploit-db, con el comando searchsploit filtraremos por Drupal 8, logrando encontrar múltiples exploits para dicha versión vulnerable, vemos uno que nos llama la atención "Drupalgeddon2", que permite un Remote Code Execution (RCE), esto en un script de ruby.

<img width="1248" height="621" alt="find6" src="https://github.com/user-attachments/assets/f8e3e777-59ff-42d3-9e87-75b11d5ba3dc" />

Lo descargaremos en nuestra máquina con el siguiente comando:

<img width="879" height="518" alt="find7" src="https://github.com/user-attachments/assets/63559b1c-092d-4122-8986-0f02c75be866" />

Lo lanzamos de la siguiente manera y vemos que ganamos acceso a la máquina víctima, pero estamos dentro de un contenedor, también vemos que nos indica revisar el directorio /shell.php y adjuntarle el parametro ?c=

<img width="837" height="622" alt="find8" src="https://github.com/user-attachments/assets/eb71167a-478b-4ddf-ad9c-3b9eec003ab7" />

Lo probamos y ¡efectivamente logramos el RCE!

<img width="840" height="631" alt="find9" src="https://github.com/user-attachments/assets/d481b867-b1a4-4c8b-86ec-f7c9fb4e903d" />

Nos pondremos en escucha con netcat por el puerto 443 para así lanzarnos una reverse shell desde la url.

<img width="677" height="241" alt="find10" src="https://github.com/user-attachments/assets/ebe15f82-7c83-453e-af7c-2da8e5d50230" />

Nos lanzamos el siguiente oneliner de reverse shell típico de bash.

<img width="682" height="240" alt="find11" src="https://github.com/user-attachments/assets/cf3399d6-8db0-4a72-9877-a9f267bd2e4d" />

Chequeamos la escucha y vemos que ¡logramos acceder a la máquina víctima!

<img width="800" height="165" alt="find12" src="https://github.com/user-attachments/assets/b4dda699-ad33-4973-858e-1b656ce59972" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```
<img width="694" height="325" alt="find13" src="https://github.com/user-attachments/assets/8a36989d-e65d-43f3-bbe0-3fab26ccda73" />

<img width="741" height="566" alt="find14" src="https://github.com/user-attachments/assets/1dcd9b5f-d570-450e-8005-00279fceff44" />

<img width="859" height="609" alt="find15" src="https://github.com/user-attachments/assets/67ae35eb-1a2c-485e-9baf-6491c95e542f" />

<img width="859" height="609" alt="find16" src="https://github.com/user-attachments/assets/441c21da-2a52-4956-8684-8633f66d3b1a" />

<img width="960" height="344" alt="find17" src="https://github.com/user-attachments/assets/bd052ebf-c44f-4dbd-86c8-4e76db104d22" />

<img width="577" height="69" alt="find18" src="https://github.com/user-attachments/assets/b2248d7a-745a-4ff9-8c92-f65a639b2d02" />

<img width="1106" height="598" alt="find19" src="https://github.com/user-attachments/assets/237461a4-1130-4fa0-8d4b-89fe39a7ad15" />

<img width="696" height="188" alt="find20" src="https://github.com/user-attachments/assets/eeaf33d4-3977-493d-ab25-5f900a398c18" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

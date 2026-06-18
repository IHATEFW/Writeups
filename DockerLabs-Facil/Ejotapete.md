La máquina Ejotapete de la plafaforma DockerLabs.es es una máquina de dificultad "Fácil", la cual nos enseña como explotar un CMS Drupal versión 8 para ganar acceso a la máquina víctima y luego por exponer passwords en archivos de configuración logramos pivotar al usuario root. .

# EJOTAPETE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="794" height="585" alt="ejota1" src="https://github.com/user-attachments/assets/2406b717-6f9e-4a92-ac59-2055332ffb64" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que solo existe el puerto 80 abierto, correspondiente al servicio HTTP.

<img width="772" height="519" alt="ejota2" src="https://github.com/user-attachments/assets/0ba0f0bc-7d42-4aa4-aa9f-c3454eb9de49" />

Una vez que ya tenemos el puerto abierto, procederemos a realizar otro escaneo con nmap, pero esta vez un poco más exhaustivo, indicandole a nmap que nos encuentre la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconomiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, que dice "Forbidden" con código de estado 403, significa que no tenemos permisos para acceder a ella, vamos a verificarla.

<img width="812" height="638" alt="ejota3" src="https://github.com/user-attachments/assets/96ab42ba-c47e-499a-a450-4d5fdb9b8f4e" />

Efectivamente nos arroja el estado 403.

<img width="828" height="406" alt="ejota4" src="https://github.com/user-attachments/assets/e6e99229-1a9c-4924-b5b1-e7c141683287" />

Haremos fuerza bruta de directorios con la herramienta gobuster, esto para poder encontrar directorios ocultos que estén despues de la /, con el siguiente comando, una vez ejecutado, podemos visualizar un directorio llamado /drupal, esto significa que posiblemente estamos ante un CMS drupal, vamos a verificarlo.

<img width="1339" height="559" alt="ejota5" src="https://github.com/user-attachments/assets/3805b413-fc35-4e1d-97b4-5bdfbd538393" />

Dentro del directorio, podemos ver que efectivamente es la página por defecto de Drupal.

<img width="1346" height="560" alt="ejota6" src="https://github.com/user-attachments/assets/2417f780-0675-4030-a48f-24aa644623d5" />

Procedemos a revisar el código fuente con CTRL + U, y encontramos que la versión es Drupal 8.

<img width="1346" height="560" alt="ejota7" src="https://github.com/user-attachments/assets/267eb941-4c20-441a-88a7-047e60d2ac2b" />

En este punto procederemos a utilizar la base de datos de exploit-db para ver si existe algun exploit para Drupal 8, pero a nivel de terminal, esto con el comando searchsploit Drupal 8 y vemos que es vulnerable a un script de ruby, que permite explotar un "Remote Code Execution".

<img width="1346" height="560" alt="ejota8" src="https://github.com/user-attachments/assets/99520ae1-8644-43b5-b663-a2c641fd0332" />

Lo descargamos con el comando searchsploit -m 44449.rb.

<img width="823" height="342" alt="ejota9" src="https://github.com/user-attachments/assets/29df034e-e1ef-418e-b857-ad253ddbe52f" />

Antes de ejecutarlo, vamos a revisar con el comando which ruby si efectivamente tenemos ruby instalado en la máquina, y efectivamente, ahora lo ejecutamos con el siguiente comando

```bash
sudo ruby 44449.rb http://172.17.0.2/drupal/
```

Pero vemos que no se puede ejecutar ya que nos falta la gema highline, vamos a proceder a instalarla con el comando

```bash
sudo gem install highline
```

<img width="1157" height="335" alt="ejota10" src="https://github.com/user-attachments/assets/12b42ebd-a280-4a24-90ff-be4a2ecc4d1d" />

## 💣 EXPLOTACIÓN

Ahora que ya tenemos todo OK, procedemos a lanzar el script y ¡Ganamos acceso a la máquina víctima! 🔥, pero nos llama la atencion que se está haciendo referencia a shell.php, donde podemos utilizar el parámetro "c" para lanzar la petición, esto mediante la URL.

<img width="1178" height="595" alt="ejota11" src="https://github.com/user-attachments/assets/e120d50b-cba1-4f0c-af38-b405187537a4" />

En otra terminal procedemos a ponernos en escucha con la herramienta netcat por el puerto que queramos.

<img width="513" height="233" alt="ejota12" src="https://github.com/user-attachments/assets/a24d4bb0-15f3-49c4-ae6c-6d8812dd01f5" />

Y lanzamos la típica reverse shell mediante la URL utilizando el parámetro "c".

<img width="843" height="182" alt="ejota13" src="https://github.com/user-attachments/assets/8c920171-bd57-4780-8485-05a95ba3f655" />

Ahora si que estamos dentro de la máquina víctima.

<img width="614" height="319" alt="ejota14" src="https://github.com/user-attachments/assets/b2029007-5eee-4af0-9e32-c303fd183ed7" />

Procedemos a realizar el tratamiento de la tty con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```

## 🔑 ESCALADA DE PRIVILEGIOS

Ya con la tty estable, vamos a revisar el archivo /etc/passwd para ver si existen más usuarios en la máquina y vemos el usuario "ballenita", por lo tanto, tendremos que pivotar a dicho usuario.

<img width="673" height="467" alt="ejota15" src="https://github.com/user-attachments/assets/04e6372b-06f5-4877-a2a0-ce5fd34028bf" />

Revisamos todos los archivos que contenía el drupal, pero no encontramos nada, por lo tanto, procedemos a buscar el archivo "settings.php" que casí siempre expone credenciales válidas, con el comando:

```bash
find / -iname "settings.php" 2>/dev/null
```

Encontramos el archivo y lo miramos, ¡Encontramos las credenciales de ballenita! 🔥.

<img width="989" height="461" alt="ejota16" src="https://github.com/user-attachments/assets/f4f7637e-ff61-4c97-8494-214c0280ab8e" />

Ya como el usuario ballenita, daremos un sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algún binario, y efectivamente podemos ejecutar /bin/ls y /bin/grep

<img width="857" height="258" alt="ejota17" src="https://github.com/user-attachments/assets/a7782ec3-2933-4523-8788-64a2d3064c60" />

Listamos el directorio /root con /bin/ls y vemos un archivo llamado secretitomaximo.txt 

<img width="774" height="280" alt="ejota18" src="https://github.com/user-attachments/assets/b1630f48-0f95-46a0-a977-a4679a179c43" />

Ahora utilizaremos el binario /bin/grep para leer dicho archivo, para eso nos dirigiremos a la web gtfobins.org, donde filtraremos con grep y copiaremos el comando.

<img width="1166" height="558" alt="ejota19" src="https://github.com/user-attachments/assets/e5c865a9-8c2f-4291-ad1a-cfb9013a4c38" />

Lo lanzamos con el usuario root y podemos leer una posible password, probamos subir a root y efectivamente podemos, máquina hackeada 🔥. .

<img width="662" height="407" alt="ejota20" src="https://github.com/user-attachments/assets/8a38deb3-46a2-4bbe-928a-15b25c9e7244" />

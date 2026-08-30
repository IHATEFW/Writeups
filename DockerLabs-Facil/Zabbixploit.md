La máquina llamada "Zabbixploit" es una máquina de dificultad "Fácil", la cual nos enseña a convertir un LFI en un RCE utilizando Wrappers de php, para así poder ejecutar comandos a nivel de sistema y lanzarnos una reverse shell para ganar acceso a la máquina víctima, una vez dentro, abusamos de permisos a nivel de sudoers para pivotar a root . 

# ZABBIXPLOIT

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="923" height="604" alt="zabbix1" src="https://github.com/user-attachments/assets/c8d9f982-1821-4ea5-af41-b18d484fe774" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, para que nos muestre todos los puertos abiertos existentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen varios puertos abiertos, el 22, 80, 3306, 10050 y 10051, correspondiente a los servicios SSH, HTTP, MySQL y Zabbix

<img width="1152" height="478" alt="zabbix2" src="https://github.com/user-attachments/assets/163598a2-94a8-4c71-b3b1-0c5943e201cb" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios que encontramos, esto de la siguiente manera, una vez ejecutado, podemos darnos cuenta que nos encontró el http-title de la web que es "Zabbix", a su vez nos confirma que estamos enfrentandonos a una BDD MariaDB.

<img width="861" height="631" alt="zabbix3" src="https://github.com/user-attachments/assets/d6a8b1ef-326f-4c34-a237-3b61b33b43b1" />

En este punto, accederemos a revisar la web del puerto 80, vemos el típico panel de autenticación de Zabbix, no tenemos credenciales válidas, probamos inyección SQL sin éxito.

<img width="1239" height="631" alt="zabbix4" src="https://github.com/user-attachments/assets/f1407c85-b10b-409d-be3c-3cc127e9828a" />

Daremos CTRL + U para revisar el código fuente y nos encontramos con un comentario/pista, indicando que podemos loguearnos con el usuario "guest" sin contraseña.

<img width="644" height="119" alt="zabbix5" src="https://github.com/user-attachments/assets/86fb2292-851e-415b-b221-d5f688d53bdd" />

Probamos:

<img width="563" height="428" alt="zabbix6" src="https://github.com/user-attachments/assets/59f846e2-7e1d-4a49-8567-19889301fa0e" />

Y ganamos acceso al dashboard de zabbix, donde se están monitoreando diversos servidores ya que vemos distintas alertas activas, indagando podemos darnos cuenta que es la versión 3.0.3 de Zabbix.

<img width="1182" height="635" alt="zabbix7" src="https://github.com/user-attachments/assets/ed48c225-f117-4b9f-8079-1d627bed2d73" />

Nos dirigiremos al apartado de "Latest data" para explotar el CVE-2016-10134 que sirve para esta versión de Zabbix.

<img width="1023" height="526" alt="zabbix8" src="https://github.com/user-attachments/assets/9fbdf74a-db37-42aa-9637-b0c5897f6293" />

Buscamos el siguiente repositorio en github, donde se explica la vulnerabilidad, donde básicamente se trata de una versión vulnerable a SQLi error-based en el latest.php, el cual nos permite ejecutar arbitrariamente comandos abusando del parametro toggle_ids, esto adjuntando los últimos 16 caracteres de nuestra cookie de sesión zbx_sessionid para también poder concatenar un Hijacking para secuestrar la cookie de sesión del usuario Administrador y así convertirnos en él.

<img width="1187" height="638" alt="zabbix9" src="https://github.com/user-attachments/assets/be3e2a69-0ac0-49ce-983a-4d8edaaa56b4" />

<img width="782" height="463" alt="zabbix10" src="https://github.com/user-attachments/assets/c7f18ac7-7cf3-4c4b-9033-166c39a8712e" />

<img width="1280" height="519" alt="zabbix11" src="https://github.com/user-attachments/assets/eeb3f718-1b75-433d-85cb-0997ffc08a78" />

Por lo tanto, ahora abriremos Burpsuite para interceptar la petición del latest.php, lo mandamos al Repetear.

<img width="806" height="583" alt="zabbix12" src="https://github.com/user-attachments/assets/de70a31d-7e8b-4d62-b830-e8c512ed6858" />

Abrimos las herramientas de desarrollador en la web, nos copiamos nuestra zbx_sessionid

<img width="785" height="415" alt="zabbix13" src="https://github.com/user-attachments/assets/f91458f9-0743-4b74-8e8b-06e2b924b634" />

## 💣 EXPLOTACIÓN

Copiamos el payload que nos indica el repositorio adjuntándole los últimos 16 caracteres de nuestra zbx_sessionid, lo lanzamos y efectivamente nos responde con un error de sintaxis, pero nos enumera un usuario válido del sistema, significa que el payload funcionó, aquí se expone el SQLi basado en errores.

<img width="1285" height="578" alt="zabbix14" src="https://github.com/user-attachments/assets/1eb6efe5-6098-45c6-98bb-757ed349b179" />

En este punto, intentaremos ejecutar el Hijacking para robarnos la cookie de Administrador, nos urlencodearemos el siguiente payload que nos permitirá enumerar las tablas existentes de la base de datos, tenemos la sospecha que en la tabla sessions se guarda la zbx_sessionid de dicho usuario, esto lo haremos en el apartado "Decode" de burpsuite.

<img width="1190" height="369" alt="zabbix15" src="https://github.com/user-attachments/assets/edbdf7a5-e509-45d7-a70d-5c538258c99f" />

Lo lanzamos y podemos ver que nos está enumerando las 4 primeras tablas de la base de datos, pero no deja enumerar todas de una.

<img width="1238" height="577" alt="zabbix16" src="https://github.com/user-attachments/assets/ddffebbd-196a-495a-b4b9-4a406704fa7e" />

Probaremos el siguiente payload indicandole que nos enumere los primeros 16 caracteres del usuario Admin en la tabla sessions.

<img width="1228" height="442" alt="zabbix17" src="https://github.com/user-attachments/assets/26a73507-3324-44e0-86f8-c9aeeff4604a" />

Lo lanzamos y ¡Efectivamente logramos obtener los primeros caracteres de zbx_sessionid de Admin!

<img width="1295" height="608" alt="zabbix18" src="https://github.com/user-attachments/assets/ec4580ae-c38d-46b5-9b93-7dc47f4a6096" />

Ahora vamos con el siguiente payload a enumerar los otros 16 caracteres.

<img width="1212" height="461" alt="zabbix19" src="https://github.com/user-attachments/assets/c81d64e7-396a-4ed7-85ff-c02d7ec87d44" />

Lo lanzamos y completamos toda la cookie de sesión de Admin.

<img width="1301" height="591" alt="zabbix20" src="https://github.com/user-attachments/assets/444c69d7-4a22-46ef-99ab-db06b657aa1e" />

<img width="746" height="319" alt="zabbix21" src="https://github.com/user-attachments/assets/c45b6363-a91a-44e6-abc5-6a04446013fa" />

La reemplazamos y recargamos la página web.

<img width="1075" height="299" alt="zabbix22" src="https://github.com/user-attachments/assets/331b0c32-9724-4b05-8cd4-096686a69ca6" />

¡Somos el usuario Zabbix Administrator!

<img width="510" height="384" alt="zabbix23" src="https://github.com/user-attachments/assets/6c20c1a9-4ff2-407e-b80f-4e36eddb1ac4" />

Iremos al apartado de Administration > Scripts para crearnos una reverse shell, el típico oneliner de bash, le damos que se ejecute en "Zabbix server", lo guardamos.

<img width="855" height="545" alt="zabbix24" src="https://github.com/user-attachments/assets/db466aff-447a-49f4-bdd0-98d24521c16f" />

Volvemos al apartado de Monitoring > Triggers y daremos click en cualquier servidor frutal, ya nos muestra nuestra reverse shell, la ejecutamos sin antes ponernos en escucha con la herramienta netcat en nuestra terminal, ¡Ganamos acceso a la máquina víctima!, realizaremos un tratamiento de la tty.

<img width="1022" height="576" alt="zabbix25" src="https://github.com/user-attachments/assets/91cd15c5-b153-4d87-8354-71f12424f291" />

<img width="721" height="454" alt="zabbix26" src="https://github.com/user-attachments/assets/717b62f7-cd06-4883-9a21-770057c145da" />

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```

## 🔑 ESCALADA DE PRIVILEGIOS

Ya dentro de la máquina víctima somos el usuario zabbix, ahora leeremos el archivo /etc/passwd para ver si existen más usuarios válidos antes de pivotar a root, y vemos que efectivamente existen los usuarios "operador" y "supervisor".

<img width="626" height="590" alt="zabbix27" src="https://github.com/user-attachments/assets/353b9113-97a8-4519-8059-d2fb2d36b9e2" />

Por lo tanto, iremos a /home donde encontramos una pista en un .txt, que nos indica que nos apresuremos ya que la tty puede morir por distintas razones.

<img width="1328" height="283" alt="zabbix28" src="https://github.com/user-attachments/assets/e6af7dec-18b6-4f0b-a750-d9808a208836" />

Dentro de /home/zabbix vemos otra pista.

<img width="609" height="185" alt="zabbix29" src="https://github.com/user-attachments/assets/11d3924b-cd58-43bc-81d9-16759e4cf1c2" />

Nos logueamos con dichas credenciales para acceder a MySQL, listamos las bases de datos y vemos la bdd zabbix, la damos use zabbix;

<img width="693" height="587" alt="zabbix30" src="https://github.com/user-attachments/assets/8f9a772c-60aa-499d-ad2d-5d0b2066f757" />

Luego listaremos las tablas con show tables; y nos muestra muchísimas tablas, la cual como indica la pista nos sirve "scripts", leeremos su contenido con select * from scripts; y nos muestra que posiblemente existe la contraseña del usuario operador en un directorio en especifico. 

<img width="1193" height="597" alt="zabbix31" src="https://github.com/user-attachments/assets/609e65c0-4bc8-4b20-921b-c801bdff2d2b" />

Lo leemos y encontramos la contraseña del usuario operador.

<img width="551" height="104" alt="zabbix32" src="https://github.com/user-attachments/assets/4fe969bf-b99c-42f3-ab1d-7f4c7d47689d" />

Nos loguemos por SSH como operador, accedemos a /home/operador y nos muestra otra pista en un archivo .txt, pero esta vez un poco más difícil de descifrarla, indicando que tendremos que mirar un servicio interno y que quizá tendremos que ocupar descriptor de archivos, tambien indica que sigamos revisando directorios comunes.

<img width="722" height="614" alt="zabbix33" src="https://github.com/user-attachments/assets/11809f31-8a40-4566-a2de-8d9df8f07403" />

Encontramos en /opt un directorio, donde se exponen 2 archivos que necesitaremos para acceder a dicho servicio interno que solo escucha en el localhost por el puerto 9001 con un token que tendremos que pasarle, como una especie de portforwarding pero através de /dev/tcp

<img width="722" height="614" alt="zabbix34" src="https://github.com/user-attachments/assets/a5e45bfa-16e9-49a4-bcf7-60fef93abc87" />

Utilizaremos la pista que nos dieron para usar el descriptor de archivo "3" para redirigir el stdout y el stdin a dicho descriptor de archivo, esto para lograr pivotar al usuario supervisor.

<img width="626" height="282" alt="zabbix35" src="https://github.com/user-attachments/assets/a89732e1-e8f5-4c84-bd3a-0f471187641a" />

Ya como el usuario supervisor, iremos a /home/supervisor para encontrar una pista nuevamente en un archivo .txt, donde indica que explotamos un Bash TCP socket mediante un descriptor de archivo.

<img width="1188" height="244" alt="zabbix37" src="https://github.com/user-attachments/assets/22bf908b-7c4f-49c7-a922-bf0d398b280a" />

Ahora daremos sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algun binario con privilegios de root, y efectivamente podemos ejecutar una shell, vemos que hace con el comando strings y nos damos cuenta que reinicia un servicio para lograr subir a root sin contraseña, se nos ocurre adjuntar un whoami para meter el binario /bin/bash y finalmente ser root, máquina hackeada . .

<img width="668" height="503" alt="zabbix36" src="https://github.com/user-attachments/assets/c92dd629-8988-4aef-b3cf-4c10150b3a9c" />

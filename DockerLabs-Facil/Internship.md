
# INTERNSHIP

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1215" height="907" alt="inter1" src="https://github.com/user-attachments/assets/b56fae1b-88b9-43be-b04e-6fa1a68d6f01" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1500" height="603" alt="inter2" src="https://github.com/user-attachments/assets/1196d1cf-2bd3-4a92-a1ee-952cc04031f1" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "GateKeeper HR | Tu Portal de Recursos Humanos".

<img width="1288" height="866" alt="inter3" src="https://github.com/user-attachments/assets/92c68a8d-4d82-46df-bf73-8a771dacb7e6" />

En este punto, lanzaremos el comando Whatweb para ver las tecnologías que se están empleando por detrás de dicha web, a su vez, ejecutaremos un ataque de fuerza bruta de directorios con la herramienta Gobuster para ver si encontramos posibles directorios ocultos, pero no encontramos nada interesante.

<img width="1905" height="907" alt="inter4" src="https://github.com/user-attachments/assets/d7a3f909-cd5f-4dd4-bf5a-2a3cf3a50a6f" />

Vamos a revisar la web y efectivamente es un portal de recursos humanos, vemos un botón para iniciar sesión pero haciendo hovering por lo botones vemos que ninguno está funcional.

<img width="1867" height="905" alt="inter5" src="https://github.com/user-attachments/assets/c7718244-1db6-497b-88c8-ad6f3bd0de2c" />

Revisaremos el código fuente por CTRL + U para ver si existe algo por detrás que no veamos a simple vista y vemos que si se está empleando el login (login modal) de autenticación pero está deshabilitado a simple vista, tambien vemos que se está haciendo referencia a un archivo llamado script.js

<img width="946" height="905" alt="inter6" src="https://github.com/user-attachments/assets/01a2c61d-c402-466b-9b75-aaa011bf4d85" />

Abriremos las devTools y podemos ver que en el CSS de la etiqueta div de HTML está en modo "none" el display de dicho login.

<img width="1555" height="397" alt="inter7" src="https://github.com/user-attachments/assets/d728f51c-720a-4f62-96f6-8155de87768a" />

La cambiaremos a "block" y automáticamente nos aparece el login de autenticación que estabamos esperando, pero al recargar la página se pierde nuevamente.

<img width="1708" height="711" alt="inter8" src="https://github.com/user-attachments/assets/9d0b443e-4fd8-42b5-83b1-e73c345cfa4e" />

En el apartado de Network vemos que se está haciendo referencia al dominio gatekeeperhr.com 

<img width="1812" height="711" alt="inter9" src="https://github.com/user-attachments/assets/ed7675f8-e62b-41a5-83f2-68bb4fb0994f" />

Le lanzaremos por la terminal una petición con curl para ver el código fuente.

<img width="1276" height="911" alt="inter10" src="https://github.com/user-attachments/assets/3ee6b1ce-b44b-4708-8f97-4bc592c1d9d8" />

Vemos que encontramos un comentario, indicando que los pasantes tienen usuarios válidos para conectarse por SSH, necesitamos ganar acceso al dashboard para ver dichos usuarios o necesitamos extraerlos de alguna forma.

<img width="1276" height="911" alt="inter11" src="https://github.com/user-attachments/assets/b3d5902a-cb73-4979-b2df-ae6fafee4afa" />

Me traigo con curl nuevamente todo el contenido del script.js que habíamos encontrado, y lo renombro gatekeeper.js en el directorio /tmp

<img width="727" height="237" alt="inter12" src="https://github.com/user-attachments/assets/97f7703e-20ae-4212-91f1-8e7d1b053a6c" />

Lo leo y necesito encontrar donde diga "Employees", vemos que se hace referencia a 2 rutas, un /lab/login y un /lab/employees.php

<img width="1770" height="792" alt="inter13" src="https://github.com/user-attachments/assets/1a7d58e5-4ae0-4030-9300-12a6ae223c06" />

<img width="1227" height="336" alt="inter14" src="https://github.com/user-attachments/assets/e1cc38fc-c1ef-467f-8f51-fb0fc524ebeb" />

Si le tiro una petición a dicho archivo vemos que nos expone todos los empleados del portal de recursos humanos, incluidos los pasantes.

<img width="768" height="890" alt="inter15" src="https://github.com/user-attachments/assets/8d71c2c3-3d90-405b-9154-1b4bd11ffb53" />

En este punto agregaré el dominio que encontramos al archivo /etc/hosts 

<img width="872" height="428" alt="inter16" src="https://github.com/user-attachments/assets/3c04abf0-9520-4385-aaa9-d8520a46a8c5" />

Lanzo nuevamente un ataque de fuerza bruta con Gobuster pero a dicho dominio y nos encuentra un directorio /spam

<img width="1905" height="868" alt="inter17" src="https://github.com/user-attachments/assets/2ffeaf78-9e07-4772-975a-52d5f712a64d" />

Lo revisamos y nos muestra un comentario con información que posiblemente sea ROT13.

<img width="730" height="436" alt="inter18" src="https://github.com/user-attachments/assets/3583f08f-fb04-4fb8-b5f3-b751bf9b3a7b" />

Identificaremos los nombres de los pasantes, encontramos a Pedro y Valentina.

<img width="661" height="700" alt="inter19" src="https://github.com/user-attachments/assets/34084de4-1846-4b40-b805-9aa0cd144693" />

Buscaremos alguna web que permita realizar un decoded del comentario en ROT13 y encontramos la contraseña de uno de esos pasantes.

<img width="1801" height="680" alt="inter20" src="https://github.com/user-attachments/assets/f3dcb9f3-c109-4ac6-99ff-1b8c1bdac6c8" />

Probamos con valentina y no funciona, probamos con pedro y ¡ganamos acceso a la máquina víctima!

<img width="1192" height="662" alt="inter21" src="https://github.com/user-attachments/assets/dcf21963-7fe8-499c-aeb9-c2a4c0853831" />

Una vez dentro de la máquina víctima, leeremos el archivo /etc/password para ver si existen más usuarios a los cuales pivotar aparte de valentina y vemos que solo existe valentina antes de llegar a root.

<img width="852" height="822" alt="inter22" src="https://github.com/user-attachments/assets/380b1e24-06be-45e9-bf0c-10482ac679cc" />

Nos encontramos la siguiente flag en el directorio de pedro.

<img width="747" height="905" alt="inter23" src="https://github.com/user-attachments/assets/41eedb7d-c517-48ea-aaf0-0a303b62e253" />

Seguimos buscando algo para pivotar y en el directorio /opt encontramos una shell que tiene permisos de escritura por parte nuestra, pero no de ejecución.

<img width="772" height="317" alt="inter24" src="https://github.com/user-attachments/assets/1124c0ed-4873-47ad-81c1-f864de427477" />

Lo editamos y le agregamos la típica reverse shell de bash ya que vemos que se ejecuta en un cron cada 45 segundos como el usuario valentina.

<img width="1452" height="902" alt="inter25" src="https://github.com/user-attachments/assets/a37e7b20-d6c0-4d57-8608-f3fd6de39ad8" />

Nos ponemos en escucha y esperamos unos segundos a que se ejecute sola la shell y finalmente somos valentina.

<img width="945" height="337" alt="inter26" src="https://github.com/user-attachments/assets/83a8e7f7-eff2-4f5e-a5ba-07fa869d8e80" />

Ejecutaremos los siguientes comandos para tener una tty estable, esto se llama tratamiento de la tty y se realiza con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```

Encontramos la siguiente flag en el directorio de valentina, que nos anima a seguir por root.

<img width="807" height="870" alt="inter27" src="https://github.com/user-attachments/assets/758ea4ac-38c3-4010-8af1-66db38f717bc" />

Aparte, encontramos una imagen, posiblemente se esté empleando esteganografía en dicha imagen, pero como en esta máquina no existen herramientas para revisar, nos tendremos que llevar dicha imagen a la máquina atacante, esto con el comando scp de la siguiente manera.

<img width="912" height="290" alt="inter28" src="https://github.com/user-attachments/assets/cc8ca164-78a6-45be-976f-133d93c0785e" />

Ya con el archivo dentro de la máquina atacante, aplicamos el comando steghide --extract -sf para extraer metadatos o alguna información en dicha imagen, logrando encontrar un secret.txt que expone una posible contraseña.

<img width="732" height="200" alt="inter29" src="https://github.com/user-attachments/assets/74ebcba6-dade-4170-893a-51b23a3e909d" />

<img width="596" height="225" alt="inter30" src="https://github.com/user-attachments/assets/d6ed1f41-8ac1-4ec1-954f-83dc41a20039" />

<img width="976" height="913" alt="inter31" src="https://github.com/user-attachments/assets/ae33e34e-c9f0-4802-a3aa-981f7b083f9b" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

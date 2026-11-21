
# PICKLE RICK

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1882" height="910" alt="rick1" src="https://github.com/user-attachments/assets/54d4d09d-a726-4fac-b657-397a1cbdbd7c" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente está encendida, a su vez le lanzamos un traceroute que nos indica que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64 que indica que nos enfrentamos a una máquina linux.

<img width="1882" height="910" alt="rick2" src="https://github.com/user-attachments/assets/435bcada-3af9-4689-be0e-9bf4d48216a8" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1872" height="649" alt="rick3" src="https://github.com/user-attachments/assets/0466da97-d910-41ad-b14c-9c119c92ac73" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el http-title que corre en la web del puerto 80, que indica "Rick is sup4r cool".

<img width="1882" height="871" alt="rick4" src="https://github.com/user-attachments/assets/0727b2c7-6610-4cde-8208-812a2c5ec011" />

## 💣 EXPLOTACIÓN

Seguiremos realizando un ataque de fuerza bruta con la herramienta Gobuster, esto para lograr identificar directorios ocultos que no sean posibles acceder a primera vista, lo lanzamos y encontramos un /login.php.

<img width="1882" height="871" alt="rick5" src="https://github.com/user-attachments/assets/32fd2448-99b8-423c-a2c9-d766ae01a7f1" />

Revisaremos la web detrás del puerto 80 y vemos que se presenta una situación, Rick ha sido convertido en pepinillo y no puede volver a su forma original, para eso le pide ayuda a Morty para encontrar 3 ingredientes secretos.

<img width="1882" height="871" alt="rick6" src="https://github.com/user-attachments/assets/71cbc924-6cf9-4b93-8049-52e33b88eed1" />

Miraremos el código fuente por CTRL + U para ver si encontramos algo por detrás y efectivamente encontramos un usuario en un comentario, lo guardamos.

<img width="1882" height="871" alt="rick7" src="https://github.com/user-attachments/assets/4ba5260a-6358-4b12-ace6-2ed5858b040c" />

Ahora nos vamos a revisar el /login.php, el cual es un tipico login que pide credenciales, probamos una inyección SQL sin éxito.

<img width="1882" height="871" alt="rick8" src="https://github.com/user-attachments/assets/c97fa47f-3ee8-48d5-8489-0a9ffb041141" />

Anteriormente, con el ataque de fuerza bruta que habíamos arrojado, tambien nos encontró un /robots.txt, lo revisamos y encontramos una posible contraseña.

<img width="1727" height="150" alt="rick9" src="https://github.com/user-attachments/assets/35a377d2-a4be-4671-a82b-5a61266c0203" />

<img width="1385" height="265" alt="rick10" src="https://github.com/user-attachments/assets/57cfa892-3fcb-4480-b6a7-1f90548b25e1" />

Nos logramos loguear con el usuario y la contraseña, ¡ganamos acceso al dashboard!

<img width="1831" height="733" alt="rick11" src="https://github.com/user-attachments/assets/193287ad-08ab-4b0a-b015-e0f263b35648" />

Revisamos el apartado "Potions" y vemos un mensaje que dice que solo el verdadero Rick puede ver esta página, miraremos el código fuente con CTRL + U y encontramos una cadena en base64, la copiamos.

<img width="1663" height="884" alt="rick12" src="https://github.com/user-attachments/assets/7faf0869-081e-482c-9313-084d494dfda7" />

<img width="1663" height="884" alt="rick13" src="https://github.com/user-attachments/assets/a32644b5-85d7-4d83-b840-1a499df52a0f" />

Intentamos decodearla y vemos que nos muestra otra cadena en base64, arrojaremos el siguiente código en python3 para que la desencripte de manera recursiva, logrando encontrar la palabra "rabbit hole", que es una trampa.

<img width="1852" height="884" alt="rick14" src="https://github.com/user-attachments/assets/9746e829-825c-4393-bb28-999115c52370" />

Volvemos al apartado "Commands" y probamos algún comando, le damos whoami y nos muestra el usuario www-data de la máquina víctima, vemos que se puede ejecutar un RCE, esto tiene buena pinta porque podremos lanzarnos una reverse shell y ganar acceso a la máquina víctima.

<img width="1832" height="754" alt="rick15" src="https://github.com/user-attachments/assets/2dc397d0-c73b-4ace-93a9-68daae3488e9" />

Preparamos la reverse shell, la típica de bash.

<img width="1832" height="754" alt="rick16" src="https://github.com/user-attachments/assets/bbb2bb36-5708-4bb3-9f43-7f7712d6e5b5" />

Nos levantamos un listening por el puerto 443 y lanzamos el comando en la web, ¡ganamos acceso a la máquina víctima!

<img width="1838" height="445" alt="rick17" src="https://github.com/user-attachments/assets/73632160-09a9-4749-9b04-818bc4b3a11b" />

Ya como el usuario red, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```

## 🔑 ESCALADA DE PRIVILEGIOS

En el mismo directorio /var/www/html encontramos el primer ingrediente en un archivo .txt

<img width="1546" height="566" alt="rick18" src="https://github.com/user-attachments/assets/b0e9779d-843d-4943-aa69-74d895eb905f" />

Lo pegamos en la web de TryHackMe.

<img width="1852" height="905" alt="rick19" src="https://github.com/user-attachments/assets/5053f2cb-ddc4-45be-9b49-d688c87bea06" />

Luego vemos otro archivo llamado clue.txt, que nos indica que sigamos revisando en los archivos para encontrar el 2do ingrediente.

<img width="802" height="107" alt="rick20" src="https://github.com/user-attachments/assets/90279fb9-bab8-46d6-a62f-7c5c12cd708b" />

En el directorio /home/rick encontramos el 2do ingrediente con éxito.

<img width="1343" height="847" alt="rick21" src="https://github.com/user-attachments/assets/22bf0fb9-6e6f-469c-ac1e-5569cf647a60" />

Lo pegamos en la web de TryHackMe, ahora nos queda solo el último ingrediente.

<img width="1888" height="889" alt="rick22" src="https://github.com/user-attachments/assets/e9fd0439-9abf-4885-80bc-01101a9de5bf" />

Arrojaremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algun binario con permisos de root y efectivamente podemos ejecutar cualquier comando como root, damos sudo su y ¡somos root!, el directorio /root encontramos el 3er ingrediente.

<img width="1732" height="765" alt="rick23" src="https://github.com/user-attachments/assets/1a6930c7-5cd6-4c1c-b5c1-3860428fcfb2" />

Lo pegamos en la web de TryHackMe y damos por hackeada la máquina con éxito . .

<img width="1857" height="901" alt="rick24" src="https://github.com/user-attachments/assets/21830f54-e554-4a9e-b730-385fae0dfa85" />

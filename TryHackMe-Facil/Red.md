# RED

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1836" height="907" alt="red1" src="https://github.com/user-attachments/assets/5ea826b3-8706-45fe-b9c5-3341cdfd8b3d" />

<img width="1836" height="907" alt="red2" src="https://github.com/user-attachments/assets/68f0a8ee-3589-4500-ab8b-7d21ce587813" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente está encendida, a su vez le lanzamos un traceroute que nos indica que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64 que indica que nos enfrentamos a una máquina linux.

<img width="1836" height="907" alt="red3" src="https://github.com/user-attachments/assets/d0ea58d8-e838-44e2-95fb-d8205d57cff4" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1836" height="907" alt="red4" src="https://github.com/user-attachments/assets/62cf87e3-c81c-4415-b796-208e2f93d09b" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el http-title que corre en la web del puerto 80, que indica "Atlanta - Free business bootstrap template".

<img width="1836" height="907" alt="red5" src="https://github.com/user-attachments/assets/602269ab-b33d-442c-91e4-21233fb31f46" />

En este punto, arrojaremos el comando whatweb, el cual actúa como un wappalyzer pero a nivel de consola, detectando todas las tecnologías que se están empleando en la web del puerto 80, vemos que se hace referencia a un /index.php?page=home.html

<img width="1833" height="445" alt="red6" src="https://github.com/user-attachments/assets/fe71fb55-49af-46ac-8f05-64a0533a2b2e" />

Ahora revisaremos la web del puerto 80, en la cual podemos visualizar que se hace referencia a una plantilla moderna y totalmente adaptable, pero nada interesante, solo un comentario que indica "Blue is a loser :p"

<img width="1877" height="873" alt="red7" src="https://github.com/user-attachments/assets/501be197-3390-4973-bbf8-95bb12e43cc4" />

Volvemos a la terminal y le lanzaremos una petición con el comando curl a la url, concatenando un wrapper en PHP para ver si podemos leer o escribir datos desde distintos orígenes o aplicar filtros de forma uniforme, esta vez lo arrojaremos para ver si podemos leer el código del /index.php, lo arrojamos y vemos cosas interesantes, vemos que se está intentando sanitizar las cadenas ../ y ./, pero al parecer la sanitización es insuficiente, lo cual posiblemente nos permitirá efectuar la vulnerabilidad de LFI.

<img width="1877" height="873" alt="red8" src="https://github.com/user-attachments/assets/2efbe698-dc31-4ad9-ad05-52ff856bb7d2" />

Intentamos leer el archivo /etc/passwd de la máquina víctima y efectivamente podemos leerlo, descubriendo 2 usuarios válidos del sistema, como red y blue.

<img width="1877" height="873" alt="red9" src="https://github.com/user-attachments/assets/ae67387b-3cb8-4fbc-bf55-03038470295c" />

<img width="1877" height="873" alt="red10" src="https://github.com/user-attachments/assets/3794aee3-d406-4bf5-992c-35bd70c228e0" />

Como podemos ejecutar un LFI, leeremos el archivo .bash_history del directorio /home/blue, para ver el historial de comandos del usuario blue, vemos que se utilizó el comando Hashcat para generar una lista de posibles contraseñas modificadas a partir del contenido del archivo .reminder  

<img width="1877" height="873" alt="red11" src="https://github.com/user-attachments/assets/79c05c82-7c0e-48d4-a87b-28a46e7da015" />

Vemos que contiene ese archivo .reminder y encontramos una contraseña.

<img width="1877" height="873" alt="red12" src="https://github.com/user-attachments/assets/a7e45169-e9af-4278-a3d2-972f977c9dfb" />

Creamos un archivo password.txt y la agregamos.

<img width="1837" height="492" alt="red13" src="https://github.com/user-attachments/assets/be213291-cd6f-4e21-b159-d89e79fbe237" />

En este punto, utilizaremos el mismo comando que encontramos de Hashcat pero le adjuntamos el archivo password.txt que creamos, redigiriremos todas las instrucciones de best64.rule a passlist.txt, una vez finaliza la ejecución del comando, leemos passlist.txt y ya tenemos combinaciones en base a la contraseña que encontramos en .reminder

<img width="1856" height="870" alt="red14" src="https://github.com/user-attachments/assets/82f67c53-a3a4-4f50-8a1d-b95a679de253" />

Como ya tenemos un diccionario de contraseñas, arrojaremos un ataque de fuerza bruta de SSH con la herramienta Hydra para intentar encontrar la password correcta del usuario blue, lo arrojamos y encontramos su password, por lo tanto, ¡ya podemos loguearnos por SSH y ganar acceso a la maquina víctima! 

<img width="1857" height="887" alt="red15" src="https://github.com/user-attachments/assets/3e7f8cf9-b633-4889-9fd4-f384ac8d995f" />

<img width="1857" height="887" alt="red16" src="https://github.com/user-attachments/assets/16536bf3-394a-4b19-ad88-a2ee71ae7124" />

Dentro de la máquina víctima, nos vamos al directorio /home/blue y encontramos la primera flag, la pegamos en la web de TryHackMe.

<img width="1857" height="887" alt="red17" src="https://github.com/user-attachments/assets/a15fa1cb-dece-4fab-a25a-80b6d6c0a020" />

<img width="1857" height="887" alt="red18" src="https://github.com/user-attachments/assets/f994732b-5257-4a4e-97df-a26b90e5c8d6" />

<img width="1784" height="882" alt="red19" src="https://github.com/user-attachments/assets/d63ebd69-6663-427e-bb95-ee9a701e186f" />

<img width="1803" height="253" alt="red20" src="https://github.com/user-attachments/assets/309e8881-b369-4bc0-8a70-2e728a244a65" />

<img width="1794" height="437" alt="red21" src="https://github.com/user-attachments/assets/92ef5a91-6b09-426a-9e0f-7e2529aaa0c8" />

<img width="1794" height="597" alt="red22" src="https://github.com/user-attachments/assets/c703e4bc-b3f0-430a-a119-d350364c2886" />

<img width="1843" height="892" alt="red23" src="https://github.com/user-attachments/assets/7d64a747-22bf-4137-9428-cd604a2e9276" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

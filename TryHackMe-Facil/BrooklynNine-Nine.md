La máquina Brooklyn Nine Nine de la plataforma TryHackMe.com es una máquina de dificultad "Fácil", pensada para principiantes y con dos caminos distintos para rootearla. En este writeup abusaremos del FTP anónimo para leer una nota que delata al usuario jake, descubriremos un archivo oculto mediante esteganografía (steghide) escondido en la imagen de la web del puerto 80, realizaremos un ataque de fuerza bruta por SSH con Hydra contra el usuario jake para ganar acceso al sistema, leeremos la flag de usuario y, finalmente, escalaremos a root abusando de un permiso sudo sobre el binario /usr/bin/less siguiendo GTFOBins . .

# BROOKLYN NINE NINE

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1899" height="897" alt="bro1" src="https://github.com/user-attachments/assets/adb90558-a646-4fac-98f1-f3b915d180c4" />

<img width="1899" height="897" alt="bro2" src="https://github.com/user-attachments/assets/1a9c838d-b2e7-4c99-8697-cd1baa166c06" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente responde. A su vez le lanzamos un ping con la opción -R (record route) que nos indica que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64, que nos indica que nos enfrentamos a una máquina linux.

<img width="1899" height="897" alt="bro3" src="https://github.com/user-attachments/assets/0d7903bb-f298-443e-91eb-88c45dcc298b" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima. Le indicamos que escanee todo el rango de puertos (-p-), que solo nos muestre los abiertos (--open), a una velocidad alta (--min-rate 5000) y mediante un escaneo SYN (-sS). Una vez ejecutado, podemos darnos cuenta de que existen los puertos abiertos 21, 22 y 80, correspondientes a los servicios FTP, SSH y HTTP.

<img width="1843" height="639" alt="bro4" src="https://github.com/user-attachments/assets/d80b8194-c62c-4418-b51a-a300ce4e27f6" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento (-sC) a su vez que nos enumere la versión de dichos servicios (-V). Una vez ejecutado, encontramos información muy jugosa: el puerto 21 corre un vsFTPd 3.0.3 que además ¡permite el login anónimo (Anonymous FTP login allowed)! y nos muestra que existe un archivo llamado note_to_jake.txt.

<img width="1855" height="901" alt="bro5" src="https://github.com/user-attachments/assets/ac7b7acb-cb53-4276-92a1-d01c84b7d55e" />

Siguiendo con la salida del mismo escaneo, podemos visualizar que el puerto 22 corre un OpenSSH 7.6p1 sobre Ubuntu y que el puerto 80 corre un servidor web Apache httpd 2.4.29 (Ubuntu).

<img width="1855" height="901" alt="bro6" src="https://github.com/user-attachments/assets/c1d83a46-9126-4bdd-8fe4-a32cdc9adede" />

Como el FTP nos permite el acceso anónimo, nos conectaremos con el comando ftp indicándole el usuario anonymous, cuando nos pida la contraseña simplemente la dejamos en blanco y presionamos enter. Una vez dentro, listamos el contenido con dir y vemos el archivo note_to_jake.txt, así que lo descargamos a nuestra máquina atacante con el comando get.

<img width="1870" height="707" alt="bro7" src="https://github.com/user-attachments/assets/7adc0f43-8be4-449b-9fab-31d0688df85a" />

Leemos el contenido de la nota que descargamos y nos encontramos con un mensaje de Amy dirigido a Jake, en el que le pide que cambie su contraseña porque es demasiado débil y que Holt se enfadará si alguien hackea la comisaría. Esto nos da una pista muy valiosa: el usuario jake posee una contraseña débil, candidata perfecta para un ataque de fuerza bruta.

<img width="1850" height="536" alt="bro8" src="https://github.com/user-attachments/assets/c679f725-a040-49a8-8f68-c98a11dd7388" />

Ahora revisaremos la web del puerto 80, en la cual nos encontramos únicamente con una imagen de fondo de la serie Brooklyn Nine-Nine ocupando toda la pantalla, sin nada más a simple vista.

<img width="1905" height="890" alt="bro9" src="https://github.com/user-attachments/assets/2cff1e3c-d0ee-4649-b05c-027b5fd7a384" />

Como no vemos nada interesante a simple vista, revisaremos el código fuente de la web (view-source) y al final del documento encontramos un comentario HTML muy revelador que dice "Have you ever heard of steganography?" (¿Alguna vez has oído hablar de la esteganografía?), lo cual nos indica claramente que hay información oculta dentro de la imagen.

<img width="1899" height="801" alt="bro10" src="https://github.com/user-attachments/assets/97fc3105-e9b1-4b3b-ba11-4e18c732748f" />

Con la ayuda de las herramientas de desarrollador del navegador (inspector), confirmamos que la imagen de fondo que se está cargando mediante CSS en el div con clase .bg es brooklyn99.jpg.

<img width="1904" height="911" alt="bro11" src="https://github.com/user-attachments/assets/be99bb77-1b34-4804-8f7e-3daaa082100e" />

## 💣 EXPLOTACIÓN

Ahora que sabemos el nombre de la imagen, la descargaremos a nuestra máquina atacante con el comando wget apuntando a http://10.66.130.26/brooklyn99.jpg para poder analizarla cómodamente de forma local.

<img width="1819" height="511" alt="bro12" src="https://github.com/user-attachments/assets/4e8a5579-9e18-488e-8692-bf842f6ce3f2" />

Antes de sacar la artillería pesada, revisaremos los metadatos de la imagen con la herramienta exiftool, por si contienen alguna pista o credencial. En este caso no encontramos nada especialmente útil, solo los datos típicos del archivo JPEG.

<img width="1810" height="872" alt="bro13" src="https://github.com/user-attachments/assets/189e4aba-ba57-4ac7-85f6-a74afe45dadd" />

Como el comentario nos habló de esteganografía, es muy probable que haya un archivo embebido protegido con contraseña. Para romper dicha contraseña usaremos la herramienta stegcracker, pasándole la imagen, y por defecto utilizará el diccionario rockyou.txt. Tras unos segundos, logra crackearla y nos revela que la contraseña de la esteganografía es admin.

<img width="1844" height="659" alt="bro14" src="https://github.com/user-attachments/assets/30b8a641-4844-40c5-8864-7244842e955c" />

Con la contraseña en mano, extraemos el archivo oculto de la imagen con steghide usando la opción --extract -sf brooklyn99.jpg. Nos genera un archivo llamado note.txt que, al leerlo, resulta contener lo que parece ser la contraseña de Holt: fluffydog12@ninenine. Este es, de hecho, uno de los dos caminos previstos para resolver la máquina.

<img width="1852" height="885" alt="bro15" src="https://github.com/user-attachments/assets/771c4bf9-6a25-4856-8ff7-cb9464284e1b" />

Intentamos autenticarnos por SSH probando esa credencial con los usuarios jake y holts, pero el servicio nos devuelve "Permission denied", por lo que este camino no nos da acceso directo por SSH. No hay problema: recordemos la nota de Amy que nos advertía que jake tenía una contraseña débil, así que tomaremos ese otro camino.

<img width="1846" height="674" alt="bro16" src="https://github.com/user-attachments/assets/537699ea-bb27-463b-b11d-7b143887344b" />

Realizaremos un ataque de fuerza bruta contra el servicio SSH con la herramienta Hydra, indicándole el usuario jake (-l jake), el diccionario rockyou.txt (-P), el protocolo ssh contra nuestra ip víctima, limitando a 4 tareas en paralelo (-T 4) y pidiéndole que se detenga en cuanto encuentre una credencial válida (-I). Lo lanzamos y al poco tiempo Hydra encuentra la contraseña del usuario jake: 987654321.

<img width="1836" height="507" alt="bro17" src="https://github.com/user-attachments/assets/fc3d02bc-af6a-4ba3-8386-633a97270f93" />

Con las credenciales ya descubiertas, nos conectamos por SSH como el usuario jake y, una vez dentro, ejecutamos whoami y hostname -I para confirmar que efectivamente somos jake y que estamos dentro de la máquina víctima. ¡Ya tenemos acceso al sistema!

<img width="1836" height="507" alt="bro18" src="https://github.com/user-attachments/assets/2944e9e5-1738-4b42-baef-3a968b2a49b1" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya dentro del sistema, leeremos el archivo /etc/passwd para enumerar los usuarios válidos que tienen una shell asignada, encontrando a amy, holt y jake, todos con /bin/bash.

<img width="1121" height="886" alt="bro19" src="https://github.com/user-attachments/assets/02b1d976-2d5f-44d0-8126-f57f8ddfbdd8" />

Buscaremos la flag de usuario por todo el sistema con el comando find / -iname "user.txt" 2>/dev/null, y la encontramos dentro del directorio /home/holt. Nos vamos hacia allí, la leemos con cat user.txt y copiamos su contenido para pegarlo en la web de TryHackMe.

<img width="1770" height="880" alt="bro20" src="https://github.com/user-attachments/assets/8ac39f0f-4ba5-44d3-ba64-09d1cc30c5a6" />

Pegamos la flag de usuario en la web de TryHackMe y nos la da como correcta.

<img width="1861" height="911" alt="bro21" src="https://github.com/user-attachments/assets/54781c6a-7a35-4d9c-bd50-1eee33f67762" />

En este punto, para la escalada de privilegios, lo primero que haremos será ejecutar sudo -l para listar qué comandos puede ejecutar nuestro usuario jake como sudo. Vemos algo muy interesante: jake puede ejecutar como cualquier usuario y SIN contraseña (NOPASSWD) el binario /usr/bin/less.

<img width="1856" height="386" alt="bro22" src="https://github.com/user-attachments/assets/ec4217b4-1e13-4512-9639-021ff7911747" />

Para saber cómo abusar de este permiso, consultaremos la página de GTFOBins, que es un repositorio que documenta cómo binarios legítimos de Unix pueden ser utilizados para saltarse restricciones de seguridad. Buscamos less y vemos que, si podemos ejecutarlo vía sudo, podemos obtener una shell interactiva simplemente abriendo un archivo con less y escribiendo !/bin/sh dentro del paginador.

<img width="1790" height="828" alt="bro23" src="https://github.com/user-attachments/assets/924662f8-daee-4fba-bee1-09c516d6aa13" />

Replicamos la técnica de GTFOBins: ejecutamos sudo -u root less /etc/hosts y, dentro del paginador de less, escribimos !/bin/sh para que nos lance una shell. Comprobamos con whoami que ¡ya somos root! Nos vamos al directorio /root, listamos su contenido y encontramos el archivo root.txt, lo leemos con cat y obtenemos la flag final de la máquina.

<img width="1790" height="828" alt="bro24" src="https://github.com/user-attachments/assets/fe65307c-0f39-4f27-8586-bb879bca353b" />

Copiamos la flag de root, la pegamos en la web de TryHackMe y listo, máquina hackeada . .

<img width="1906" height="902" alt="bro25" src="https://github.com/user-attachments/assets/ac456d72-52de-4ea6-bca7-726ecd89e0c9" />

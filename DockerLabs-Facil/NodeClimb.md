
# NODECLIMB

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1100" height="864" alt="node1" src="https://github.com/user-attachments/assets/57fe45ab-6bc8-461b-926d-64b947c5391a" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 21 y 22, correspondientes a los servicios FTP y SSH.

<img width="1505" height="604" alt="node2" src="https://github.com/user-attachments/assets/1e8cce6c-45e9-4a3a-88e3-6e6d470434c0" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar que nos permite loguearnos con el usuario Anonymous sin contraseña, a su vez, un archivo .zip el cual se encuentra dentro del puerto 21, el puerto 22 lo descartaremos de momento ya que no tenemos credenciales válidas.

<img width="1261" height="904" alt="node3" src="https://github.com/user-attachments/assets/0a4cdf66-ce2f-4dd1-9574-b4cad247d23b" />

Nos loguearemos con el usuario Anonymous por el puerto 21, una vez dentro, nos traemos a nuestra máquina atacante el archivo .zip con el comando "get".

<img width="1340" height="793" alt="node4" src="https://github.com/user-attachments/assets/b34137c9-1b02-428b-a78c-a5bb13ae267b" />

## 💣 EXPLOTACIÓN

En este punto, lo intentaremos descomprimir con el comando "unzip" pero está protegido con contraseña, para esta situación, utilizaremos la herramienta Fcrackzip para realizar un ataque de fuerza bruta con algún diccionario para intentar encontrar dicha contraseña, una vez ejecutado nos encuentra la contraseña para descomprimirlo, la utilizamos y podemos visualizar un archivo .txt con unas credenciales posiblemente válidas para loguearnos por SSH.

<img width="1450" height="793" alt="node5" src="https://github.com/user-attachments/assets/0cea4bff-4aa9-4f43-be57-aa18387f8d39" />

Nos logueamos por SSH y ¡ganamos acceso a la máquina víctima!

<img width="1180" height="605" alt="node6" src="https://github.com/user-attachments/assets/f96e3cbf-ebd2-4867-9657-5a2f5787646e" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro, daremos el comando sudo -l para ver si podemos ejecutar algún binario con permisos a nivel de sudoers y vemos que podemos ejecutar /usr/bin/node para lanzar un script.js que existe en el home de mario, leemos dicho script pero no tiene contenido, pero como está en nuestro home tenemos permisos de escritura.

<img width="1293" height="605" alt="node7" src="https://github.com/user-attachments/assets/5d70b761-b7d8-4e9d-af72-797539edb779" />

Se nos ocurre inyectarle código malicioso javascript para lanzarnos una bash privilegiada, esto de la siguiente manera:

<img width="901" height="306" alt="node8" src="https://github.com/user-attachments/assets/47d7c3e9-a54b-403f-acff-f2df77a23f45" />

Guardamos y lo ejecutamos como root, posterior a esto, ya somos root, máquina hackeada . .

<img width="1274" height="662" alt="node9" src="https://github.com/user-attachments/assets/a898bffa-8a5b-48b5-8cbe-f755f1e2660f" />

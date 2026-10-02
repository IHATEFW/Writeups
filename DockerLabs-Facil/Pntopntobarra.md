
# PNTOPNTOBARRA

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1189" height="883" alt="pnto1" src="https://github.com/user-attachments/assets/b704d20c-b80e-4948-969d-3169671577b1" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1496" height="602" alt="pnto2" src="https://github.com/user-attachments/assets/c94c1379-4776-43dd-bb05-d5fa17ee72cf" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "Advertencia: LeFvIrus".

<img width="1397" height="866" alt="pnto3" src="https://github.com/user-attachments/assets/15369379-4945-43fe-b524-c0c97fa4b014" />

En este punto, arrojaremos el comando Whatweb para que me detecte las tecnologías que se están empleando detrás de dicha web, a su vez, lanzaremos un ataque de fuerza bruta de directorios con la herramienta Gobuster, esto para encontrar posibles directorios ocultos, una vez ejecutado, podemos visualizar un /index.php

<img width="1899" height="866" alt="pnto4" src="https://github.com/user-attachments/assets/66210893-775b-4ff5-bde9-a683af83e385" />

Revisamos la web y nos arroja un mensaje que dice que nuestra máquina está infectada y que actuemos ahora.

<img width="1899" height="866" alt="pnto5" src="https://github.com/user-attachments/assets/4d0954d2-e93c-47a4-8f84-acdd1c0df885" />

## 💣 EXPLOTACIÓN

Clickearemos en el botón que dice "Ejemplos de computadoras infectadas" y está haciendo referencia al archivo ejemplos.php, concatenando el parámetro ?images, por lo tanto, se nos ocurre intentar efectuar un LFI (Local File Inclusion), para intentar leer el archivo /etc/passwd, el cual conseguimos con éxito, podemos ver el usuario "nico" válido del sistema. 

<img width="1899" height="866" alt="pnto6" src="https://github.com/user-attachments/assets/01d668e6-d3bb-4ba2-a173-d6f8b5a7df57" />

Realizaremos un ataque de fuerza bruta de SSH al usuario nico para poder encontrar su contraseña, pasandole el diccionario de contraseñas rockyou.txt, pero sin éxito.

<img width="1566" height="467" alt="pnto7" src="https://github.com/user-attachments/assets/afea9993-9e5b-4e31-9c42-bfb3282f14ae" />

Como no nos queda otra, secuestraremos el id_rsa del usuario nico, para poder intentar loguearnos sin que nos pida contraseña.

<img width="991" height="913" alt="pnto8" src="https://github.com/user-attachments/assets/7962ac84-5417-4b63-8760-933ef869749a" />

La pegamos en un archivo que llamaremos id_rsa, le daremos permisos 600 y nos conectaremos con ssh -i id_rsa nico@172.17.0.2

<img width="1184" height="892" alt="pnto9" src="https://github.com/user-attachments/assets/d2faca5f-d2f9-4a93-8e3d-8776d946ee54" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya dentro de la máquina víctima, daremos el comando sudo -l para ver si podemos ejecutar algún binario con permisos a nivel de sudoers, y efectivamente podemos ejecutar /bin/env, nos lanzamos una shell privilegiada y somos root, máquina hackeada . .

<img width="1286" height="626" alt="pnto10" src="https://github.com/user-attachments/assets/8fa76786-dac9-4537-beec-f64c71a0188f" />

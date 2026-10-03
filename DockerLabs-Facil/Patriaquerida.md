La máquina Patriquerida de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como podemos explotar un LFI con un parámetro que encontramos realizando fuzzing, logrando leer el archivo /etc/passwd y luego leer un archivo en un directorio oculto que se expone en el index.php encontrando una contraseña válida, una vez dentro de la máquina víctima pivotamos entre los usuarios gracias a credenciales expuestas y permisos SUID hasta llegar a root ..

# PATRIAQUERIDA

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1234" height="912" alt="patria1" src="https://github.com/user-attachments/assets/432afdfd-d332-4e16-82ca-64e55dbc7187" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1506" height="608" alt="patria2" src="https://github.com/user-attachments/assets/6888d6fc-6bdb-4c14-8ccc-bd1efa9c5792" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar que la página web que corre en el puerto 80 es la típica web por defecto de Apache2.

<img width="1498" height="904" alt="patria3" src="https://github.com/user-attachments/assets/006db39d-57a2-4c45-a59f-209a929e78a6" />

## 💣 EXPLOTACIÓN

En este punto, vamos a realizar un ataque de fuerza bruta con la herramienta Gobuster, esto para poder identificar directorios ocultos, una vez ejecutado, podemos visualizar que nos encontró un /index.php

<img width="1904" height="904" alt="patria4" src="https://github.com/user-attachments/assets/38b58923-341e-4c28-8769-f0bc0cf621f1" />

Esta es la página web que encontramos en el index.html

<img width="1904" height="904" alt="patria5" src="https://github.com/user-attachments/assets/0bbea23e-3483-47b4-b6ea-a87f10c19e65" />

Nos vamos al /index.php y vemos que hace referencia a un directorio /var/www/html/.hidden_pass, indica que no olvidemos visualizar el archivo oculto.

<img width="1386" height="386" alt="patria6" src="https://github.com/user-attachments/assets/046172de-664a-4ba5-8053-f5f0acea01d0" />

Como tenemos el /index.php, vamos a ver si podemos concatenar un parámetro para lograr explotar algun LFI o un RCE, esto lo haremos con la herramienta Wfuzz, para hacer fuzzing de dicho parámetro, una vez ejecutado, podemos ver que encontramos el parámetro "page".

<img width="1911" height="693" alt="patria7" src="https://github.com/user-attachments/assets/25911d33-0b64-4c81-93e0-164b4577166b" />

Abusaremos de dicho parámetro para lograr explotar un LFI y leer el archivo /etc/passwd, y efectivamente podemos lograr leer usuarios válidos del sistema, encontramos el usuario mario y el usuario pinguino.

<img width="1913" height="414" alt="patria8" src="https://github.com/user-attachments/assets/bd4e2b3d-f20a-47f3-9b6b-b45c2f89595a" />

Como ya podemos explotar un LFI, vamos a leer el archivo oculto que nos indicaban hace un rato, logrando encontrar una posible password "balu".

<img width="919" height="253" alt="patria9" src="https://github.com/user-attachments/assets/e4f70a8e-e527-4986-9d03-c4e1b37708b5" />

Probamos conexión por SSH con el usuario pinguino y password balu, ¡ganando acceso a la máquina víctima!

<img width="957" height="772" alt="patria10" src="https://github.com/user-attachments/assets/2477e49a-bcd1-4a69-841f-9896236a1cdd" />

## 🔑 ESCALADA DE PRIVILEGIOS

En el directorio /home/pinguino existe un archivo .txt que expone la password del usuario mario, logramos pivotar a mario.

<img width="799" height="588" alt="patria11" src="https://github.com/user-attachments/assets/c5f15059-38f0-4e34-a95c-75dd7ad31b48" />

Ya como el usuario mario, daremos el siguiente comando para ver si podemos ejecutar algun binario con permisos SUID y efectivamente podemos ejecutar python3 como el usuario root.

<img width="799" height="588" alt="patria12" src="https://github.com/user-attachments/assets/4c712f6b-2d03-4105-972a-787fc81f2971" />

Nos vamos a la web gtfobins.org y filtramos por "python", nos vamos al apartado "SUID" y copiamos el primer comando.

<img width="1443" height="886" alt="patria13" src="https://github.com/user-attachments/assets/1b6250e5-2d63-4be1-99c3-24f955dbdd2e" />

Lo ejecutamos y finalmente somos root, máquina hackeada . .

<img width="1002" height="796" alt="patria14" src="https://github.com/user-attachments/assets/571b23a5-2a17-49c4-a1b7-0d24135e18a6" />


# ELEVATOR

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1048" height="666" alt="ele1" src="https://github.com/user-attachments/assets/fbdfc1a1-8d43-4c5f-9944-00c037a2040a" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existe solamente el puerto 80 abierto.

<img width="1532" height="574" alt="ele2" src="https://github.com/user-attachments/assets/1a19387d-63ec-4d31-8d8e-a79ccbd61aa1" />

Una vez ya tenemos el puerto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio, una vez ejecutado, podemos darnos cuenta que encontramos el http-title de la web, llamada "El Ascensor Embrujado - Un Misterio de Scooby-Doo"

<img width="1340" height="701" alt="ele3" src="https://github.com/user-attachments/assets/57c13426-79e8-492e-8bb2-40fd6bbf7423" />

## 💣 EXPLOTACIÓN

Nos vamos a revisar la web, donde aparece un ascensor con un botón que dice "¡Abre el Ascensor!".

<img width="1601" height="873" alt="ele4" src="https://github.com/user-attachments/assets/85142305-8faa-4a0f-a27b-6597a3386194" />

Una vez presionamos el botón, nos arroja el siguiente mensaje, sobre una historia de Scooby-Doo.

<img width="1601" height="873" alt="ele5" src="https://github.com/user-attachments/assets/26678a98-1817-4136-8086-fbe861f6cb2f" />

Como ya no tenemos nada más que hacer en dicha web, nos vamos a la terminal, para seguir enumerando, esta vez ejecutaremos la herramienta dirb para poder encontrar directorios ocultos, una vez ejecutado, nos llamada la atención un directorio llamado /themes

<img width="982" height="880" alt="ele6" src="https://github.com/user-attachments/assets/ab0b0be8-cea1-4607-b9ba-47402e55e547" />

Pero no tenemos permisos para ver que hay dentro de él.

<img width="1202" height="549" alt="ele7" src="https://github.com/user-attachments/assets/19b79a9a-1da6-4f63-acd5-58d9566b5e62" />

Ahora ocuparemos la herramienta Gobuster para seguir enumerando directorios ocultos, pero el ataque de fuerza bruta lo lanzaremos a dicho directorio /themes, encontrando 2 archivos interesantes, un /archivo.html y un /upload.php

<img width="1903" height="895" alt="ele8" src="https://github.com/user-attachments/assets/7429e5fe-d5cd-4a89-8994-bd7652b618c4" />

Revisamos el /archivo.html y vemos que se puede subir una imagen en formato JPG, se nos ocurre subir una reverse shell en php pero la renombraremos revshell.php.jpg para ver si deja subirla.

<img width="1903" height="895" alt="ele9" src="https://github.com/user-attachments/assets/25c1b2c3-489d-4840-b3e0-34e1f57f4042" />

Nos vamos a la web revshells.com para configurar nuestra reverse shell con la ip de nuestra máquina atacante y un puerto que escojamos. 

<img width="1903" height="895" alt="ele10" src="https://github.com/user-attachments/assets/709ea7b0-7973-4d81-8aad-4f6068719340" />

La copiamos en un archivo llamado revshell.php.jpg y la guardamos.

<img width="1152" height="390" alt="ele11" src="https://github.com/user-attachments/assets/75b3cee0-0a21-4f1f-84d0-15f7b61e31e4" />

La logramos subir con éxito a la web.

<img width="1152" height="390" alt="ele12" src="https://github.com/user-attachments/assets/386f650c-8536-45a2-8a1f-8733d7047ee7" />

Nos vamos al directorio /themes/uploads y la vemos arriba.

<img width="1057" height="445" alt="ele13" src="https://github.com/user-attachments/assets/cc745f8d-7fe0-47e2-8c42-3cfbb8c8dd48" />

Nos ponemos en escucha con la herramienta Netcat con el puerto 443 y presionamos dicho archivo en la web, ¡Ganamos acceso a la máquina víctima!

<img width="1307" height="499" alt="ele14" src="https://github.com/user-attachments/assets/37376d96-ff5c-4ca2-9161-c852c80efb38" />

Ejecutaremos los siguientes comandos para tener una tty estable, esto se llama tratamiento de la tty y se realiza con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```
## 🔑 ESCALADA DE PRIVILEGIOS

Ya con una tty estable, daremos el comando sudo -l para ver si podemos ejecutar algún binario con privilegios de otro usuario e ir pivotando hasta llegar a root, vemos que podemos ejecutar /usr/bin/env como el usuario daphne, lo ejecutamos de la siguiente manera y somos daphne.

<img width="1307" height="499" alt="ele15" src="https://github.com/user-attachments/assets/3e3b25f5-632c-4d42-bd6a-fe0928189455" />

Damos nuevamente sudo -l y vemos que podemos ejecutar /usr/bin/ash como el usuario vilma, lo ejecutamos de la siguiente manera y somos vilma.

<img width="1307" height="499" alt="ele16" src="https://github.com/user-attachments/assets/9fb09172-f7a8-4bce-b0f8-230a0d82fc6d" />

Daremos nuevamente sudo -l y vemos que podemos ejecutar /usr/bin/ruby como el usuario shaggy, lo ejecutamos de la siguiente manera y somos shaggy.

<img width="1302" height="486" alt="ele17" src="https://github.com/user-attachments/assets/7768fd64-e1c8-41f3-ad43-bfea56581fb1" />

Damos nuevamente sudo -l y vemos que podemos ejecutar /usr/bin/lua como el usuario fred, lo ejecutamos de la siguiente manera y somos fred.

<img width="1302" height="486" alt="ele18" src="https://github.com/user-attachments/assets/08a200d9-79e8-4dd8-bf74-fe0ea4b69d7b" />

Damos nuevamente sudo -l y vemos que podemos ejecutar /usr/bin/gcc como el usuario scooby, lo ejecutamos de la siguiente manera y somos scooby.

<img width="1302" height="486" alt="ele19" src="https://github.com/user-attachments/assets/f6b97390-3c4d-4031-bd8c-6125e0413766" />

Finalmente damos una vez más sudo -l y vemos que podemos ejecutar /usr/bin/sudo como el usuario root, lo ejecutamos de la siguiente manera y somos root, máquina hackeada . .

<img width="1302" height="486" alt="ele20" src="https://github.com/user-attachments/assets/19462c79-1f44-4897-bc29-aa1a6d140ec1" />

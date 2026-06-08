La máquina Los 40 Ladrones de la plataforma DockerLabs.es, es de dificultad Fácil, la cual nos enseña la técnica Port Knocking, que realiza un "golpeteo de puertos", logrando acceder a puertos que visualmente estaban cerrados. .

# LOS 40 LADRONES

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="941" height="562" alt="los40ladrones1" src="https://github.com/user-attachments/assets/d209f987-1ead-45c3-bf47-9baa6a0cc166" />

## 🔎 ENUMERACIÓN

Cuando ya tengamos la ip de la máquina víctima, se procederá a realizar un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos exístentes abiertos/expuestos que tenga la máquina, esto con el siguiente comando, una vez ejecutado, podemos visualizar que tenemos solamente el puerto 80 abierto, relacionado al servicio HTTP.

<img width="941" height="562" alt="los40ladrones2" src="https://github.com/user-attachments/assets/f62adb18-877e-403b-a307-c553baefbd04" />

Ya sabemos que está el puerto 80 abierto, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que al parecer la web que hay por detrás es la típica página de Apache2 por defecto, vamos a revisarla.

<img width="999" height="636" alt="los40ladrones3" src="https://github.com/user-attachments/assets/a3b6bb2e-ffa0-40c2-8ead-f6a5580ecb70" />

<img width="1161" height="635" alt="los40ladrones4" src="https://github.com/user-attachments/assets/5a57e7d3-3e46-472f-96d0-39725f9cf942" />

Revisamos el código fuente con CTRL + U, pero no vemos nada interesante, en este punto procederemos a realizar fuerza bruta de directorios con la herramienta gobuster, para ver que directorios exísten detrás de la /, de la siguiente manera:

<img width="1339" height="595" alt="los40ladrones5" src="https://github.com/user-attachments/assets/2d0b024b-c69f-4170-92f0-1b5fced78db9" />

Nos encontró un archivo .txt llamado qdefense.txt, procederemos a revisarlo, vemos que nos está dando la pista de un posible usuario llamado "toctoc", y nos entrega unos posibles puertos, el 7000, 8000 y 9000, perfecto, realizaremos una técnica que se llamada Port Knocking, que realiza un "golpe secreto" o "golpeteo de puertos" para abrir puertos secuencialmente que se cerraron por seguridad, una vez la máquina detecta que se efectuó la combinación de puertos adecuada, los abre, esto de la siguiente manera:

<img width="733" height="247" alt="los40ladrones6" src="https://github.com/user-attachments/assets/fbf0fe2d-b27b-4e33-ab32-abe79ea5224b" />

## 💣 EXPLOTACIÓN

Ya lanzado el comando, volveremos a realizar un escaneo de puertos abiertos, para verificar que algun puerto se haya abierto. Vemos que efectivamente se apertuŕó el puerto 22 de SSH, por lo tanto, ahora con el posible usuario que encontramos "toctoc", realizaremos fuerza bruta de SSH con la herramienta hydra, para encontrar la contraseña, de la siguiente manera:

<img width="829" height="589" alt="los40ladrones7" src="https://github.com/user-attachments/assets/d5636bc1-a0bb-4422-a0ac-1931d3300eb1" />

<img width="951" height="622" alt="los40ladrones8" src="https://github.com/user-attachments/assets/81205e12-746a-42af-a6cf-f46289e11358" />

Accedemos por SSH con la password encontrada como muestra la evidencia anterior, ¡y ya ganamos acceso a la máquina víctima!

## 🔑 ESCALADA DE PRIVILEGIOS

Ya dentro de la máquina víctima, daremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers, y efectivamente podemos ejecutar el binario /opt/bash como el usuario root, daremos el comando sudo -u root /opt/bash ¡y ya somos root!, máquina hackeada.

<img width="1031" height="466" alt="los40ladrones9" src="https://github.com/user-attachments/assets/65f3cf3f-dac1-4061-815e-7471a9e8831d" />

La máquina Trust de la plataforma dockerlabs.es, es de dificultad "Muy Fácil" y es una de las primeras máquinas que recomiendo realizar cuando estas iniciando en el mundo del hacking/pentesting . .

# TRUST

🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="726" height="544" alt="maquinatrust1" src="https://github.com/user-attachments/assets/f7c9decb-5d8f-4668-adda-d8f0a63f5bb0" />

🔎 ENUMERACIÓN

Tendremos en nuestras manos la ip de la máquina víctima, en este caso la 171.21.0.2, por lo tanto, procedemos a enumerar los puertos abiertos/expuestos con nmap:

<img width="1140" height="789" alt="maquinatrust2" src="https://github.com/user-attachments/assets/c5d5b952-8707-4789-83dd-b844b7ad9f56" />

Vemos que está expuesto el puerto 22 relacionado al servicio ssh y el puerto 80 relacionado al servicio http, seguiremos enumerando un poco más exhaustivo, solicitando a nmap que enumere las versiones de dichos servicios y que arroje un conjunto básico de script de reconocimiento:

<img width="1533" height="788" alt="maquinatrust3" src="https://github.com/user-attachments/assets/653129a0-fa0e-4178-be74-1c51411a966c" />

Se procede a acceder al servicio http, encontrando la página por defecto de Apache2, se inspecciona el código fuente con CTRL + U pero no se encuentra nada importante:

<img width="1525" height="788" alt="maquinatrust4" src="https://github.com/user-attachments/assets/55929186-0754-4034-8b1f-63943ec21d82" />

En este punto, intentaremos aplicar fuerza bruta de directorios con la herramienta gobuster, con la finalidad de encontrar directorios luego del /, lanzaremos el siguiente comando:

<img width="1294" height="589" alt="maquinatrust5" src="https://github.com/user-attachments/assets/41306be6-22a1-4bfd-81f2-8c48110aa2dc" />

Hemos encontrado un directorio llamado /secret.php, se procede a acceder a él, vemos que existe un posible usuario válido llamado "Mario".

<img width="1514" height="788" alt="maquinatrust6" src="https://github.com/user-attachments/assets/761755b4-5f1a-417a-875e-819fd0f0b6cf" />

💣 EXPLOTACIÓN

Una vez que ya tenemos un posible usuario válido, se realizará fuerza bruta de ssh con la herramienta hydra, para intentar encontrar la contraseña del usuario "Mario", esto empleando un diccionario de contraseñas, el típico rockyou.txt

<img width="1352" height="345" alt="maquinatrust7" src="https://github.com/user-attachments/assets/ab185750-1411-4034-ba4f-c18ecdc6002b" />

¡Hemos encontrado la contraseña válida!, en este caso "chocolate", por lo tanto, ganamos acceso a la máquina víctima.

<img width="1345" height="402" alt="maquinatrust8" src="https://github.com/user-attachments/assets/2aff00ee-7363-4725-8838-2fd839f81de4" />

🔑 ESCALADA DE PRIVILEGIOS

Ya dentro de la máquina víctima, lo primero que realizaremos es ver si tenemos permisos a nivel de sudoers, con el comando sudo -l

<img width="1328" height="277" alt="maquinatrust9" src="https://github.com/user-attachments/assets/1c0bf15c-749f-4d22-9b51-acbb9fa124b2" />

Vemos que sí tenemos permisos para ejecutar el binario /usr/bin/vim como el usuario root, ya casí lo tenemos, finalmente procederemos a recurrir a la página gtfobins.org, la cual es vital para ejecutar comandos para pivotar de usuarios, filtramos por vim y nos indica los comandos a ejecutar, o simplemente ejecutamos el comando sudo -u root vim y dentro del editor presionamos ESC + :, y le damos !/bin/bash, en este caso yo ejecuté el comando sudo -u root vim -c ':!/bin/sh' ya somos root, máquina hackeada,

<img width="477" height="107" alt="maquinatrust10" src="https://github.com/user-attachments/assets/665afcb7-c89d-461b-924d-6c6fb3fc9029" />

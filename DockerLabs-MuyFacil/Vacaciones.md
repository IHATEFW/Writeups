Séptima máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina expone una página web con un mensaje oculto, haciendo referencia a 2 posibles usuarios válidos, uno de ellos es el correcto . .

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1305" height="342" alt="vacaciones1" src="https://github.com/user-attachments/assets/1aa24a21-4486-4239-9f3b-fc1a3a127345" />

## 🔎 ENUMERACIÓN

Comenzaremos a resolver esta máquina utilizando la herramienta nmap para enumerar los puertos abiertos, esto nos permitirá escoger alguna vía de ingreso a nuestra máquina víctima, lo realizaremos con el siguiente comando, una vez ejecutado, podemos visualizar que encontramos los puertos 22 y 80 abiertos, relacionados a los servicios SSH y HTTP:

<img width="1510" height="780" alt="vacaciones2" src="https://github.com/user-attachments/assets/bf806d9a-e180-4991-afb8-a3ca504ac0ba" />

Una vez identificados los puertos abiertos, procederemos a seguir enumerando con la herramienta nmap, pero esta vez, indicandole que nos encuentre las versiones que corren detrás de esos servicios, como tambien, indicandole que nos arroje un conjunto básico de scripts de reconocimientos:

<img width="1505" height="792" alt="vacaciones3" src="https://github.com/user-attachments/assets/637737af-0a54-486a-9822-309de7bde2de" />

Podemos darnos cuenta que nos enumeró la versión de SSH OpenSSH 7.6, la cual es vulnerable a una enumeración de usuarios, hasta la versión 7.7 de OpenSSH se puede ejecutar un exploit que nos permita la enumeración de usuarios válidos dentro de la máquina víctima, lo cual es grave de cara a un atacante, en este caso como la máquina "BreakMySSH" se explotó de la misma manera, ahora procederemos a irnos por el puerto 80, por lo tanto, abriremos el navegador y pondremos la ip de la máquina víctima, encontrando una página en blanco (en este caso en negro ya que mi tema es oscuro jaja).

<img width="1526" height="784" alt="vacaciones4" src="https://github.com/user-attachments/assets/52bc617c-d2b8-4c3a-8f55-9c003e148bc5" />

No existe nada a simple vista, pero si inspeccionamos el código fuente con CTRL + U, podremos ver una pista clave:

<img width="906" height="301" alt="vacaciones5" src="https://github.com/user-attachments/assets/ab42814b-5778-4bda-a690-4d17c0082415" />

Un pequeño mensaje para Camilo de Juan, lo cual nos da el indicio que podremos encontrar la contraseña de un posible usuario válido Camilo con hydra, realizando un ataque de fuerza bruta de SSH, procederemos a ello:

## 💣 EXPLOTACIÓN

Con la herramienta hydra, encontramos la contraseña del usuario Camilo, esto ejecutando fuerza bruta con el diccionario rockyou.txt, procederemos a ganar acceso a la máquina víctima:

<img width="1512" height="433" alt="vacaciones6" src="https://github.com/user-attachments/assets/c1c1a1e0-bd19-4876-8774-e78efbed9667" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina víctima, veremos si tenemos permisos a nivel de sudoers, pero vemos que no, por lo tanto, procederemos a dirigirnos al directorio /var/mail, esto recordando el mensaje que encontramos en la web, que indicaba que Juan le dejó un correo a Camilo. Una vez dentro, encontramos un directorio llamado Camilo, entramos y vemos un archivo .txt que hace referencia a la password de Juan, en este punto logramos pivotar al usuario Juan.

<img width="1525" height="738" alt="vacaciones7" src="https://github.com/user-attachments/assets/43b9d7a5-8bac-4eea-b5d7-ad5bfebcf9f0" />

Ya con usuario Juan, damos el comando "sudo -l" para ver si tenemos permisos sudoers, y efectivamente podemos ejecutar el binario /usr/bin/ruby como el usuario root.

<img width="1457" height="245" alt="vacaciones8" src="https://github.com/user-attachments/assets/0737cb9e-8cf3-4da5-b079-d21aaa6b6525" />

Por último, nos dirigiremos a la página web gtfobins.org, y filtraremos por "Ruby", logrando encontrar el comando correcto para elevar privilegios a root con Ruby, finalmente somos root, ¡máquina hackeada!

<img width="1504" height="512" alt="vacaciones9" src="https://github.com/user-attachments/assets/76331d60-93e7-494e-b880-a1cd6565ee3e" />

<img width="618" height="75" alt="vacaciones10" src="https://github.com/user-attachments/assets/99db43c8-1938-4583-ac9a-8493d5a6965b" />





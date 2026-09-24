La máquina Pinguinazo de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como explotar una vulnerabilidad del OWASP top 10, llamada SSTI (Server-Side Template Injection), la cual nos permite ejecutar un RCE para lanzarnos una reverse shell y ganar acceso a la máquina víctima, luego dentro, logramos pivotar a root abusando Java a nivel de sudoers . .

# PINGUINAZO

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1170" height="837" alt="pingu1" src="https://github.com/user-attachments/assets/a9d45218-b5c8-46f7-b43c-cd1b344b8c0e" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que solo existe el puerto 5000 abierto, correspondiente al servicio UPnP (Universal Plug and Play).

<img width="1517" height="578" alt="pingu2" src="https://github.com/user-attachments/assets/85b901f4-7056-4509-8c4c-0de6fc12e6fe" />

Una vez ya tenemos el puerto abierto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio, esto de la siguiente manera, una vez ejecutado, podemos visualizar que está corriendo Werkzeug de Python, relacionado a una web, donde tambien podemos identificar su titulo, llamado "Pingu Flask Web"

<img width="1242" height="704" alt="pingu3" src="https://github.com/user-attachments/assets/943d4023-1f8c-4eb0-9231-c95c2c271caf" />

Lanzamos el comando Whatweb para detectar las tecnologias que se están empleando.

<img width="1885" height="342" alt="pingu4" src="https://github.com/user-attachments/assets/d24ad960-f7ea-4e75-b513-ad05946cdc9e" />

Ahora revisaremos la web detrás del puerto 5000, vemos un campo de registro, donde nos solicitan algun nombre, cumpleaños, email, etc.

<img width="1385" height="673" alt="pingu5" src="https://github.com/user-attachments/assets/b64f4bf6-8696-4620-b26e-616a15203ac3" />

## 💣 EXPLOTACIÓN

Ingresamos el nombre Prueba y le damos "Save all", vemos que se está interpretando y apareciendo de la misma manera cualquier información que ingresemos.

<img width="743" height="291" alt="pingu6" src="https://github.com/user-attachments/assets/5c5dea24-900b-4df2-8fda-7785d1e1ed06" />

En este punto, vemos que podemos explotar un SSTI (Server-Side Template Injection), una vulnerabilidad típica del OWASP top 10, la cual se explota de la siguiente manera, probaremos inyectar un 7*7 de la siguiente manera, debiese darnos como resultado el numero 49.

<img width="1173" height="692" alt="pingu7" src="https://github.com/user-attachments/assets/fc3c7ddc-615f-4501-a699-38dfa607b0c8" />

Efectivamente nos responde la multiplicación.

<img width="697" height="283" alt="pingu8" src="https://github.com/user-attachments/assets/a11190c9-81de-47f9-8066-64d6b7909a3a" />

Vamos a probar este payload que incorpora el comando "id", para que nos muestre el usuario activo de la máquina víctima.

<img width="1212" height="794" alt="pingu9" src="https://github.com/user-attachments/assets/61fcb380-d5b2-4556-be11-aa44263371e3" />

Nos muestra el usuario "pinguinazo", hemos logrando colar un RCE, vamos por buen paso.

<img width="940" height="267" alt="pingu10" src="https://github.com/user-attachments/assets/95b65b3b-f020-4c8d-bc41-786dafad2f8b" />

Utilizaremos el mismo payload, pero en esta ocasión nos lanzaremos una reverse shell típica de bash, por el puerto 443, esto con la finalidad de ganar acceso a la máquina víctima.

<img width="1262" height="783" alt="pingu11" src="https://github.com/user-attachments/assets/fbee7a01-5211-466b-b018-ee632fb855ec" />

Nos ponemos en escucha con la herramienta netcat por el puerto 443, y lanzamos la reverse shell, ¡Ganamos acceso a la máquina víctima!

<img width="889" height="377" alt="pingu12" src="https://github.com/user-attachments/assets/a96f5176-c53c-4a74-8f60-8b6a65d1a841" />

## 🔑 ESCALADA DE PRIVILEGIOS

Leeremos el archivo /etc/passwd para ver si existen más usuarios válidos en el sistema aparte de pinguinazo y root, pero vemos que solamente tendremos que pivotar a root.

<img width="868" height="776" alt="pingu13" src="https://github.com/user-attachments/assets/2cbca4dc-010a-40da-adec-e6e5fa3ca9c7" />

Damos un sudo -l, para ver si tenemos privilegios a nivel de sudoers para ejecutar algun binario como el usario root, y efectivamente podemos ejecutar Java.

<img width="1429" height="340" alt="pingu14" src="https://github.com/user-attachments/assets/986b5dc5-09a8-4f8c-b98d-5c2fa4d4c3ca" />

Nos vamos a la web gtfobins.org y filtramos por la palabra Java, nos copiaremos el primer payload que hace que nos lancemos una /bin/sh, para luego compilar un Shell.java

<img width="1478" height="881" alt="pingu15" src="https://github.com/user-attachments/assets/a2b2fd81-bd02-47a1-b25b-d4e7fa87534e" />

Nos vamos al directorio /tmp ya que tenemos permisos de escritura en dicho directorio, lanzamos el payload, lo compilamos con javac, y lo ejecutamos como root, finalmente ¡Ganamos acceso a la máquina como root!, máquina hackeada . .

<img width="828" height="581" alt="pingu16" src="https://github.com/user-attachments/assets/b43a0c69-fbaf-4a1b-b9dd-2af6eb3d2516" />

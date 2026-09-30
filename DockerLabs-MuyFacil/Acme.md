
# ACME

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="933" height="695" alt="acme1" src="https://github.com/user-attachments/assets/bce584fc-3c95-4e8e-8424-6aea339f3a33" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1509" height="601" alt="acme2" src="https://github.com/user-attachments/assets/999e79b2-08ec-4270-8df6-bcb2f9159926" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "ACME Corporation - Portal de Mantenimiento", tambien vemos que está habilitado el /robots.txt

<img width="1293" height="911" alt="acme3" src="https://github.com/user-attachments/assets/dbdada4e-a9a6-4eec-af17-dc9c1de737f3" />

## 💣 EXPLOTACIÓN

En este punto, vamos a realizar un ataque de fuerza bruta con la herramienta Gobuster para encontrar posibles directorios ocultos de la máquina víctima y vemos que al finalizar solo encontramos la web que corre detrás del puerto 80 que hace referencia al /index.html y el /robots.txt

<img width="1537" height="856" alt="acme4" src="https://github.com/user-attachments/assets/424cacdd-b546-4e9b-a026-91ea58789593" />

Revisando la web, podemos ver una especie de portal, que está en mantenimiento, el cual nos indica que podemos ingresar por SSH para consultar el aviso y las notificaciones del sistema con cualquier usuario que le otorguemos.

<img width="1543" height="897" alt="acme5" src="https://github.com/user-attachments/assets/432f1e55-a8b3-479c-9887-57cb89f93381" />

Antes de ingresar, encontramos un /migration_notes.txt, el cual no tiene más información que lo mismo que nos dicen en el portal.

<img width="771" height="320" alt="acme6" src="https://github.com/user-attachments/assets/ae4d864f-0fd2-4590-9868-1beb19d5b685" />

Ahora si probamos acceso por SSH con cualquier usuario, yo le pasé el usuario "prueba" y de inmediato me arrojó credenciales válidas para tareas de mantenimiento, me logueo con dichas credenciales y ¡ganamos acceso a la máquina víctima!

<img width="931" height="907" alt="acme7" src="https://github.com/user-attachments/assets/f15a3c79-24cc-4c46-b509-0b2553df81e0" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro, lanzamos el siguiente comando para ver si tenemos algun binario que podamos ejecutar con permisos SUID y efectivamente existe /usr/bin/bash y /usr/bin/dash, ambos binarios explotables para lanzarnos una shell privilegiada, damos cualquiera de los dos comandos (bash -p o dash -p) y ¡somos root!

<img width="593" height="741" alt="acme8" src="https://github.com/user-attachments/assets/96a548ca-cff3-4668-9b29-366db1f2b237" />

Ya como el usuario root, nos vamos al directorio /root y encontramos la flag, máquina hackeada . .

<img width="713" height="295" alt="acme9" src="https://github.com/user-attachments/assets/17ce8332-1bff-4a4b-a835-aadb99fa7255" />

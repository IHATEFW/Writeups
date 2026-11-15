La máquina Flynn de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como podemos encontrar un usuario válido detrás de la web del puerto 80, del cual nos aprovechamos realizando un ataque de fuerza bruta con Hydra para encontrar su password, una vez dentro de la máquina víctima, pivotamos a root abusando de un permiso a nivel de sudoers mal configurado.

# FLYNN

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1508" height="886" alt="fly1" src="https://github.com/user-attachments/assets/a8113d1a-3b34-400d-8277-ea54c7e7f1e5" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1744" height="886" alt="fly2" src="https://github.com/user-attachments/assets/93914567-74b3-4040-bee6-94138a766802" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "Site Doesn't have a title", pero no nos guiaremos por eso y la revisaremos.

## 💣 EXPLOTACIÓN

<img width="1744" height="886" alt="fly3" src="https://github.com/user-attachments/assets/3eb86fc6-4f1b-488e-8d13-4aa1676ea6fd" />

Una vez dentro, podemos visualizar un nombre llamado "Flynn", además de una imagen, la cual podemos descargar para ver si se está empleando esteganografía 

<img width="1744" height="886" alt="fly4" src="https://github.com/user-attachments/assets/ca2f56ec-8a86-40d0-9b4e-dcbbabbdd7f9" />

<img width="1301" height="402" alt="fly5" src="https://github.com/user-attachments/assets/3f30dcec-c549-4175-b0f7-43377202f7c7" />

La traemos a nuestra máquina con el comando wget.

<img width="1676" height="425" alt="fly6" src="https://github.com/user-attachments/assets/bc2d6236-3299-4ff2-a041-81e264a334e2" />

Ejecutamos algunas técnicas de esteganografía pero no tuvimos resultado exitoso, por lo tanto, como tenemos un usuario posiblemente válido, ejecutaremos un ataque de fuerza bruta con la herramienta Hydra para encontrar su password, lanzándole un diccionario de posibles contraseñas similar al de la máquina Tron de dockerlabs, una vez lo ejecutamos podemos darnos cuenta que encontramos la password.

<img width="1697" height="857" alt="fly7" src="https://github.com/user-attachments/assets/8de97589-7900-4ed8-ace2-9251ae0cede3" />

¡Ganamos acceso a la máquina víctima!

<img width="1697" height="857" alt="fly8" src="https://github.com/user-attachments/assets/0406242d-875e-4665-8d30-026df3ae6ddf" />

## 🔑 ESCALADA DE PRIVILEGIOS

Le damos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers para ejecutar algún binario y vemos que podemos ejecutar /usr/bin/env, lo ejecutamos de la siguiente manera y somos root, máquina hackeada . .

<img width="1697" height="857" alt="fly9" src="https://github.com/user-attachments/assets/8490ee7a-09b4-4048-8fb0-afeb2cff8c71" />

La máquina Tron de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como podemos abusar de una imagen que encontramos en la web del puerto 80 logrando ejecutar esteganografía, nos enumera un usuario válido y nos damos cuenta que la máquina está ambientada en la película TRON, nos creamos nuestro diccionario de contraseñas y encontramos la password del usuario tron, una vez dentro, pivotamos a root mediante permisos a nivel de sudoers . .

# TRON

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1271" height="765" alt="tron1" src="https://github.com/user-attachments/assets/65df4919-e42e-40ed-a1a6-cd51326267cc" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1545" height="609" alt="tron2" src="https://github.com/user-attachments/assets/4b77ddd1-91c0-4865-94ec-08a38fac2071" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "TRON".

<img width="1280" height="754" alt="tron3" src="https://github.com/user-attachments/assets/89c9fbe1-c89f-4c36-a554-6985b5c370bf" />

Revisamos la web del puerto 80 y vemos una imagen, con el titulo "LEGACY".

<img width="1515" height="878" alt="tron4" src="https://github.com/user-attachments/assets/c07f6bcd-fd95-4082-8ffd-8701148a68e7" />

Le damos CTRL + U para ver si existe algo por detrás que no estemos viendo y solo vemos la imagen tron.jpg, posiblemente se esté empleando esteganografía.

<img width="1149" height="653" alt="tron5" src="https://github.com/user-attachments/assets/3fb20145-a24b-4196-9bd3-6e3b0e5cedb1" />

## 💣 EXPLOTACIÓN

La traemos a nuestro directorio actual y ejecutamos la herramienta Steghide de la siguiente manera, nos extrae un disco.txt, lo leemos y nos enumera un posible usuario válido "quorra".

<img width="1542" height="696" alt="tron6" src="https://github.com/user-attachments/assets/491dc513-7e83-47bf-8340-bdcf0552b657" />

Nos damos cuenta que al hacer fuerza bruta con la herramienta Hydra por SSH, no nos deja utilizar el diccionario rockyou.txt, por lo tanto, nos creamos nuestro propio diccionario de contraseñas, pero todo ambientado en la película TRON (nos ayudamos de internet).

<img width="924" height="788" alt="tron7" src="https://github.com/user-attachments/assets/377ea346-d287-4f28-be0c-92504d1541ab" />

Ejecutamos la fuerza bruta contra el usuario tron y encontramos su password.

<img width="1907" height="908" alt="tron8" src="https://github.com/user-attachments/assets/26864b7a-754c-4fb2-803f-a16dcdd66104" />

¡Ganamos acceso a la máquina víctima!

<img width="953" height="678" alt="tron9" src="https://github.com/user-attachments/assets/d94112ac-cc10-4c1a-9138-0808623b2668" />

## 🔑 ESCALADA DE PRIVILEGIOS

Finalmente le damos el comando sudo -l para ejecutar algun binario con permisos a nivel de sudoers y vemos que podemos ejecutar /usr/bin/env, lo ejecutamos de la siguiente manera y somos root, máquina hackeada . .

<img width="1398" height="704" alt="tron10" src="https://github.com/user-attachments/assets/4c2c4931-be18-4064-a3d3-6796efe029b5" />

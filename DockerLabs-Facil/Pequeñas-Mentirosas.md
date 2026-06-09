La máquina Pequeñas-Mentirosas de la plataforma DockerLabs.es, es una máquina de dificultad Fácil, la cual nos muestra como mediante ataques de fuerza bruta SSH podemos encontrar la password de 2 usuarios y llegar al que tiene permisos a nivel de sudoers y conseguir pivotar a root. . 

# PEQUEÑAS-MENTIROSAS

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="830" height="605" alt="pequenas1" src="https://github.com/user-attachments/assets/736afe6f-a6b9-4ff8-b9bb-773818ba2b35" />

## 🔎 ENUMERACIÓN

Ya tenemos la ip de la máquina víctima, por lo tanto, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para descubrir los puertos abiertos/expuestos exístentes, con el fin de abusar de ellos, esto lo realizaremos de la siguiente manera, una vez ejecutado el escaneo, podemos visualizar que hay dos puertos abiertos, el 22 y el 80, relacionados a SSH y HTTP.

<img width="830" height="546" alt="pequenas2" src="https://github.com/user-attachments/assets/1c9a0a93-ad92-4d26-9a1b-40ee9a6e312a" />

Ya sabemos que exísten los puertos 22 y 80 abiertos, ahora volveremos a realizar un escaneo con nmap pero de forma más exhaustiva, indicandole a nmap que nos enumere la versión de dichos servicios y que nos arroje un conjunto básico de scripts de reconocimiento, esto de la siguiente manera, una vez ejecutado, vemos que la versión de SSH es un OpenSSH 9.2, la cual no es vulnerable a una enumeración de usuarios y como no tenemos credenciales válidas, nos iremos por el puerto 80, dicho puerto, tiene una web por detŕás que indica que no tiene titulo aparentemente, vamos a revisarla.

<img width="845" height="640" alt="pequenas3" src="https://github.com/user-attachments/assets/96d095cf-547d-486a-ac9f-d336802542ee" />

<img width="950" height="351" alt="pequenas4" src="https://github.com/user-attachments/assets/7acf2e94-00c7-468e-af83-99db52aabd17" />

## 💣 EXPLOTACIÓN

Dentro de la web, vemos que tenemos una pista de un posible usuario válido llamado "A", exáminaremos el código fuente con CTRL + U, pero no vemos nada interesante, en este punto, procederemos a realizar un ataque de fuerza bruta de SSH con la herramienta hydra, esto al posible usuario para intentar encontrar la contraseña, esto de la siguiente manera:

<img width="959" height="371" alt="pequenas5" src="https://github.com/user-attachments/assets/cb6693b0-4ae5-4bc9-a94f-c9dddc50fd98" />

¡Ya tenemos la contraseña del usuario "A"! 🔥, nos loguearemos por SSH con las credenciales.

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina víctima, daremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers, pero no tenemos, miraremos el archivo /etc/passwd para ver si exísten más usuarios, y nos damos cuenta que exíste el usuario "spencer", por lo tanto, deberemos pivotar a dicho usuario.

<img width="637" height="539" alt="pequenas6" src="https://github.com/user-attachments/assets/715ee3af-4318-4271-83bd-27ef687e47b4" />

Seguiremos revisando directorios que son básicos y que siempre debemos revisar, como /tmp, /opt, /srv, y este último tiene la ruta /srv/ftp donde exísten varios archivos sospechosos, uno en particular nos dice la pista de realizar fuerza bruta SSH para encontrar la password de "spencer", procederemos a realizar lo indicado desde otra terminal.

<img width="785" height="630" alt="pequenas7" src="https://github.com/user-attachments/assets/c9893bc4-22bf-4c84-949d-e56a77c9c8c8" />

¡Encontramos la password de el usuario "spencer"! 🔥, procedemos a pivotar a spencer, y le damos sudo -l para ver si podemos ejecutar algún comando como root.

<img width="933" height="364" alt="pequenas8" src="https://github.com/user-attachments/assets/5c44a34a-b103-4f2e-b73e-b8bfea35f6bf" />

<img width="944" height="305" alt="pequenas9" src="https://github.com/user-attachments/assets/8699e9c2-549c-4c54-8529-241bf8ffabc9" />

Efectivamente podemos ejecutar comandos de python3 como el usuario root, nos vamos a la página gtfobins.org y filtramos por python, nos copiamos el siguiente comando y subimos privilegios máximos con el siguiente comando.

<img width="1049" height="456" alt="pequenas10" src="https://github.com/user-attachments/assets/f7658d51-de15-416d-acf9-355221fd5f6a" />

<img width="912" height="409" alt="pequenas11" src="https://github.com/user-attachments/assets/dc495a2a-416c-4baf-8b77-eae0110f71cf" />

¡Finalmente somos root! 🔥, máquina hackeada. . 

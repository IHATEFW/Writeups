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

## 🔑 ESCALADA DE PRIVILEGIOS

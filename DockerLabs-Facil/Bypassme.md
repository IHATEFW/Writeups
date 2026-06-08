# BYPASSME

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1188" height="618" alt="bypassme1" src="https://github.com/user-attachments/assets/e1386ff7-6028-404f-a0a0-cd44c720ae39" />

## 🔎 ENUMERACIÓN

Una vez ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para conocer los puertos abiertos/expuestos que posee dicha máquina, esto se realizará con el siguiente comando, una vez ejecutado, podemos ver que exísten los puertos abiertos 22 y 80, relacionados a los servicios SSH y HTTP.

<img width="1188" height="618" alt="bypassme2" src="https://github.com/user-attachments/assets/212a041e-f826-4ac2-b80f-7d15a9b71f12" />

Ahora que ya conocemos los puertos abiertos, seguiremos utilizando la herramienta nmap para seguir enumerando, esta vez indicandole a nmap que nos encuentre la versión de dichos servicios y además, que nos arroje un conjunto básico de scripts de reconomiento, esto de la siguiente manera:

<img width="1193" height="638" alt="bypassme3" src="https://github.com/user-attachments/assets/df840e10-5f72-4347-96d2-fa5a1b1b5a4a" />

Podemos visualizar la versión de estos servicios SSH y HTTP, para el caso de SSH no tenemos credenciales válidas y la versión es superior a la 7.7, por lo tanto, no podemos enumerar posibles usuarios válidos, entonces, nos vamos con el puerto 80, vemos que exíste una web detrás que es un posible panel de auntenticación, vamos a revisarlo.-

Vemos que efectivamente estamos frente a un login, intentamos la típica inyección SQL con el comando admin' OR 1=1;-- -, pero no funciona.

<img width="1193" height="638" alt="bypassme4" src="https://github.com/user-attachments/assets/9d1a9d8f-65d4-467f-bfc0-7f2fa5dba5fc" />

Seguimos inyectandole cáracteres raros y probamos este comando en el campo de la password:

```bash
'1'='1 -- -
```

¡Logramos acceso al panel!

<img width="1193" height="638" alt="bypassme5" src="https://github.com/user-attachments/assets/f58c2047-97f5-4978-a48b-e3972a693bcb" />

Lo primero que se me ocurre es exáminar el código fuente con CTRL + U y encontramos una pista, un posible archivo logs.txt

<img width="728" height="502" alt="bypassme6" src="https://github.com/user-attachments/assets/ba749835-e7ca-4c77-840b-7b393933eac8" />

Intenté ponerlo en la URL, pero no funciona, falta un directorio previo, por lo tanto, procederemos a realizar fuerza bruta con la herramienta gobuster para que nos encuentre directorios detrás de la /, esto de la siguiente manera, una vez ejecutado, vemos que nos encontró un directorio llamado /logs

## 💣 EXPLOTACIÓN

<img width="1347" height="502" alt="bypassme7" src="https://github.com/user-attachments/assets/536ef91c-d33b-4120-83e4-dfd28db1a4ea" />

Ahora si logramos acceder a esta URL http://172.17.0.2/index.php?page=/logs/logs.txt, donde se exponen las credenciales de un usuario llamado "albert".

<img width="1005" height="619" alt="bypassme8" src="https://github.com/user-attachments/assets/89200d94-a785-47cb-b332-3e626103b654" />

Como anteriormente vimos que exíste el puerto 22 SSH abierto, con esas credenciales ¡logramos ganar acceso a la máquina víctima!

<img width="931" height="468" alt="bypassme9" src="https://github.com/user-attachments/assets/fb31fd3b-ca56-4071-a7b1-01e6852d7d00" />

## 🔑 ESCALADA DE PRIVILEGIOS

Ya dentro de la máquina víctima

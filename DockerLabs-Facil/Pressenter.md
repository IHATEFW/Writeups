
# PRESSENTER

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="747" height="616" alt="pressenter1" src="https://github.com/user-attachments/assets/b0691f01-081f-46f5-a8eb-74dc1a2d7ff7" />

## 🔎 ENUMERACIÓN

Como primero paso, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para lograr identificar los puertos abiertos/expuestos de la máquina víctima, esto con el siguiente comando, una vez ejecutado, podemos visualizar que existe el puerto 80 abierto, relacionado al servicio HTTP.

<img width="768" height="516" alt="pressenter2" src="https://github.com/user-attachments/assets/2987e0f4-60ad-41d2-b360-9cc00f8abb01" />

Una vez ya tenemos identificado el único puerto abierto, seguiremos enumerando con nmap, pero esta vez con un escane más exhaustivo, indicandole a nmap que nos encuentra la versión de dicho servicio, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, esto con el siguiente comando, una vez ejecutado, podemos ver que el titulo de la web es "Pressenter CTF", vamos a revisar dicha web.

<img width="807" height="636" alt="pressenter3" src="https://github.com/user-attachments/assets/3452d6e8-e000-4a67-8f5c-dd43162b6bda" />

<img width="1243" height="636" alt="pressenter4" src="https://github.com/user-attachments/assets/2bd43a0c-3d45-4d2a-a71c-a865e2666bcf" />

<img width="1229" height="279" alt="pressenter5" src="https://github.com/user-attachments/assets/d6416418-2889-4685-898d-37b1015e2636" />

<img width="786" height="330" alt="pressenter6" src="https://github.com/user-attachments/assets/ff960d10-0e5d-462c-81ee-7285acdd1a61" />

<img width="1223" height="628" alt="pressenter7" src="https://github.com/user-attachments/assets/3155b533-59af-4367-90bb-7440e0d08b76" />

<img width="1115" height="573" alt="pressenter8" src="https://github.com/user-attachments/assets/d6e65442-806d-4da8-9317-fbd2479f328a" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS


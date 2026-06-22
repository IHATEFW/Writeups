
# PARADISE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="641" height="612" alt="paradise1" src="https://github.com/user-attachments/assets/5d811302-9002-44ba-a672-c250bae15142" />

## 🔎 ENUMERACIÓN

El primer paso para comprometer esta máquina, será enumerar los puertos con la herramienta nmap, esto para lograr identificar posibles puertos abiertos/expuestos que tenga la máquina víctima, una vez ejecutado el primer escaneo, vemos que se visualizan tres puertos abiertos, el 22, 139 y 445, correspondientes a SSH y SMB.

<img width="841" height="612" alt="paradise2" src="https://github.com/user-attachments/assets/e7af8c70-5ce2-43e8-b724-892a2aa9978e" />

Una vez ya logramos saber los puertos abiertos, seguiremos utilizando la herramienta nmap para escanear nuevamente, pero esta vez, indicandole a nmap que nos encuentre la versión de dichos servicios, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, una vez ejecutado, podemos visualizar que el servicio SSH tiene la versión OpenSSH 6.6.1, la cual es vulnerable a una enumeración de usuarios del sistema, despues de la versión 7.7 esta vulnerabilidad fue parchada.

<img width="992" height="634" alt="paradise3" src="https://github.com/user-attachments/assets/fa3d14b3-8204-41f0-9f37-f5e68c9c9582" />

Como ya sabemos que procederemos a enumerar usuarios del sistema, podemos usar un diccionario de usuarios que vienen en parrot o kali, pero antes, procederemos a enumerar usuarios con la herramienta enum4linux ya que tenemos el servicio SMB abierto, una vez ejecutado de la siguiente manera, podemos ver que nos muestra los usuarios andy y lucas.

<img width="992" height="634" alt="paradise4" src="https://github.com/user-attachments/assets/16b1e13a-84c7-412a-9079-83bf39250599" />

<img width="992" height="634" alt="paradise5" src="https://github.com/user-attachments/assets/c1db72e5-dabf-4368-86c0-a3aa54b678fc" />

Como ya tenemos usuarios válidos, procederemos a realizar fuerza bruta SSH de contraseñas con la herramienta hydra, esto de la siguiente manera, una vez ejecutado, podemos visualizar que se encontró la password del usuario lucas.

<img width="1008" height="547" alt="paradise6" src="https://github.com/user-attachments/assets/15f2dff9-d4ea-41bc-848b-bdd5cd8ac74f" />

<img width="993" height="434" alt="paradise7" src="https://github.com/user-attachments/assets/b374179f-2fae-4386-b87e-62ed84d04d0d" />

<img width="1160" height="527" alt="paradise8" src="https://github.com/user-attachments/assets/8185e063-6e57-466b-97d7-0261d8b70168" />

<img width="1160" height="527" alt="paradise9" src="https://github.com/user-attachments/assets/d6c419d5-6ffe-4ed9-a500-815fb70a7bc1" />

<img width="458" height="422" alt="paradise10" src="https://github.com/user-attachments/assets/3e609917-94ee-479a-a86a-4ee9c5405b43" />

<img width="1332" height="612" alt="paradise11" src="https://github.com/user-attachments/assets/098ecae5-0f07-4278-ae46-3243790c7693" />

<img width="523" height="337" alt="paradise12" src="https://github.com/user-attachments/assets/270c2890-1183-4f12-9b60-18bd3e8bdb48" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

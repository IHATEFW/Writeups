La máquina WinFake de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como podemos lograr acceder a la máquina víctima con pistas en el código fuente de la página web del puerto 80 . .

# WINFAKE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="814" height="628" alt="win1" src="https://github.com/user-attachments/assets/b74f091a-24e9-41bc-883e-33d73face9d1" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, realizaremos un escaneo con la herramienta nmap, para que nos arroje todos los puertos abiertos existentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen los puertos 22 y 80 abiertos, relacionados a SSH y HTTP.

<img width="1236" height="628" alt="win2" src="https://github.com/user-attachments/assets/1c609951-e0e6-45c3-a2e6-d9a3aab6e493" />

Seguiremos enumerando con la herramienta nmap, pero esta vez indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios que están corriendo, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la web, llamada "TechWorld Noticias".

<img width="1236" height="628" alt="win3" src="https://github.com/user-attachments/assets/d0b25cc5-5b52-454b-94c4-6139767bfb9c" />

Revisaremos y vemos que nos muestra una especie de web de noticias de tecnología, pero nada importante de primera vista.

<img width="1236" height="628" alt="win4" src="https://github.com/user-attachments/assets/f823dc30-cbf8-4716-8200-6f0c6c5f03fe" />

Miraremos el código fuente con CTRL + U para ver si existe algo que se nos escape y efectivamente logramos encontrar un posible usuario válido del sistema "pipe".

<img width="616" height="457" alt="win6" src="https://github.com/user-attachments/assets/6575d9f7-3b83-4d96-8939-295f0a48ebaa" />

## 💣 EXPLOTACIÓN

Procederemos a realizar un ataque de fuerza bruta de SSH con la herramienta Hydra, utilizando el diccionario de confianza rockyou.txt, despues de unos minutos logramos encontrar la password del usuario pipe.

<img width="1217" height="391" alt="win7" src="https://github.com/user-attachments/assets/7b192422-2c22-4505-be23-bba66b31bb5d" />

Accedemos vía SSH y ¡logramos entrar a la máquina víctima! 🔥, pese a que es un linux, se simula el la típica terminal de cmd o powershell de Windows.

## 🔑 ESCALADA DE PRIVILEGIOS

<img width="740" height="626" alt="win8" src="https://github.com/user-attachments/assets/44716486-0fb9-4ba2-9a0f-eda8b8b258bb" />

En la web, en el codigo fuente, logramos identificar una pista, que indica que estamos frente a un acróstico, si juntamos todas las iniciales de todas las noticias tech, logramos unir una posible contraseña. 

<img width="884" height="558" alt="win5" src="https://github.com/user-attachments/assets/510c60c6-8184-47fc-a3f9-884ec01892f0" />

La guardamos en un .txt

<img width="469" height="208" alt="win9" src="https://github.com/user-attachments/assets/a41e1698-163e-499e-bf0c-7148422a9a3e" />

La utilizamos para probarla pivotando al usuario root y ¡Efectivamente logramos acceder al usuario de máximos privilegios root!, máquina hackeada 🔥. .

<img width="499" height="341" alt="win10" src="https://github.com/user-attachments/assets/2a1c0aae-3221-4a61-8883-13be19d6f5ca" />

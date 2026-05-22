Octava máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina expone una página web con un mensaje oculto, haciendo referencia a 2 posibles usuarios válidos, uno de ellos es el correcto . .

## OBSESSION

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1521" height="589" alt="obsession1" src="https://github.com/user-attachments/assets/38410c70-7b97-4d15-9901-ed6252ada843" />

## 🔎 ENUMERACIÓN

Como primer paso, procederemos a enumerar los puertos abiertos que pueda tener la máquina víctima, esto para saber que servicio comprometer de cara a ganar acceso a la máquina, lo realizaremos con la herramienta nmap, de la siguiente manera:

<img width="1521" height="781" alt="obsession2" src="https://github.com/user-attachments/assets/317f1032-fe99-4f21-8dd7-462f80eeb863" />

Ya efectuado el primer escaneo, nos arroja que existen los puertos 21, 22 y 80 abiertos, relacionados a los servicios FTP, SSH y HTTP, por lo tanto, procederemos a seguir enumerando con la herramienta nmap, esta vez indicandole que nos descubra la versión de dichos servicios y que nos arroje un conjunto básico de scripts de reconocimiento, lo lanzamos y nos muestra información importante, en este caso podemos darnos cuenta que el servicio FTP acepta loguearnos con el usuario Anonymous, y nos muestra ciertos archivos críticos, por lo tanto, lo primero que haremos es descargarnos esos archivos para ver que contiene:

<img width="1532" height="788" alt="obsession3" src="https://github.com/user-attachments/assets/8800367a-11f2-4b97-bdaa-7e31f9774bd3" />

Nos logueamos por FTP con el usuario Anonymous y nos descargamos los archivos:

<img width="1526" height="792" alt="obsession4" src="https://github.com/user-attachments/assets/4c880771-cb17-456b-a643-ae17c252493f" />


## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

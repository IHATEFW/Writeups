Sexta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil"

# HEDGEHOG

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1516" height="594" alt="hedgehog1" src="https://github.com/user-attachments/assets/9f57c466-c938-4c28-9055-a2af0d3c73e2" />

## 🔎 ENUMERACIÓN

Como primer paso, efectuaremos un escaneo de puertos con la herramienta nmap, esto para detectar posibles puertos abiertos que estén expuesto en la máquina víctima, esto con el siguiente comando, una vez ejecutado, podemos visualizar que están abiertos los puertos 22 y 80, correspondientes a SSH y HTTP:

<img width="1524" height="788" alt="hedgehog2" src="https://github.com/user-attachments/assets/625195e9-5a15-4249-a61c-47b09900d8c0" />

Una vez que ya tenemos identificados los puertos abiertos, procederemos a seguir enumerando con la herramienta nmap, esta vez indicandole que nos descubra la versiòn de estos servicios, a su vez, que nos arroje un conjunto básico de scripts de reconocimiento:

<img width="1521" height="784" alt="hedgehog3" src="https://github.com/user-attachments/assets/289b8b9d-1b88-46a6-b803-c58f7ac65768" />

Ya identificadas las versiones de estos servicios, nos damos cuenta que de primera impresión no existe nada importante, por lo tanto, procederemos a revisar todo lo que exista disponible en el puerto 80, abriremos el navegador y pondremos la ip de la máquina víctima, a su vez, en paralelo, ejecutaremos fuerza bruta de directorios con la herramienta gobuster, para ver si existen directorios disponibles detrás del /.

<img width="1525" height="781" alt="hedgehog4" src="https://github.com/user-attachments/assets/a08219f3-1352-4343-8861-22c6033d0438" />

<img width="1280" height="388" alt="hedgehog5" src="https://github.com/user-attachments/assets/d5f4bc44-bcd5-4663-b645-f048ec118df2" />

En base a la fuerza bruta de directorios, no encontramos nada interesante, pero en la página web podemos visualizar una especie de nombre, que es "tails", por lo tanto, podemos intuir que es posiblemente un usuario válido, entonces en este punto realizaremos fuerza bruta de ssh con hydra para encontrar la contraseña válida:





Sexta máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina expone en la página web del puerto 80, un usuario válido, del cual se aprovecha para ganar acceso a la máquina víctima . .

# HEDGEHOG

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1516" height="594" alt="hedgehog1" src="https://github.com/user-attachments/assets/9f57c466-c938-4c28-9055-a2af0d3c73e2" />

## 🔎 ENUMERACIÓN

Como primer paso, efectuaremos un escaneo de puertos con la herramienta nmap, esto para detectar posibles puertos abiertos que estén expuestos en la máquina víctima, esto con el siguiente comando, una vez ejecutado, podemos visualizar que están abiertos los puertos 22 y 80, correspondientes a SSH y HTTP:

<img width="1524" height="788" alt="hedgehog2" src="https://github.com/user-attachments/assets/625195e9-5a15-4249-a61c-47b09900d8c0" />

Una vez que ya tenemos identificados los puertos abiertos, procederemos a seguir enumerando con la herramienta nmap, esta vez indicándole que nos descubra la versión de estos servicios, a su vez, que nos arroje un conjunto básico de scripts de reconocimiento:

<img width="1521" height="784" alt="hedgehog3" src="https://github.com/user-attachments/assets/289b8b9d-1b88-46a6-b803-c58f7ac65768" />

Ya identificadas las versiones de estos servicios, nos damos cuenta que de primera impresión no existe nada importante, por lo tanto, procederemos a revisar todo lo que exista disponible en el puerto 80, abríremos el navegador y pondremos la ip de la máquina víctima, a su vez, en paralelo, ejecutaremos fuerza bruta de directorios con la herramienta gobuster, para ver si exísten directorios disponibles detrás del /.

<img width="1525" height="781" alt="hedgehog4" src="https://github.com/user-attachments/assets/a08219f3-1352-4343-8861-22c6033d0438" />

<img width="1280" height="388" alt="hedgehog5" src="https://github.com/user-attachments/assets/d5f4bc44-bcd5-4663-b645-f048ec118df2" />

En base a la fuerza bruta de directorios, no encontramos nada interesante, pero en la página web podemos visualizar una especie de nombre, que es "tails", por lo tanto, podemos intuir que es posiblemente un usuario válido 🔥, entonces en este punto realizaremos fuerza bruta de ssh con hydra para encontrar la contraseña válida:

## 💣 EXPLOTACIÓN

Vemos que el escaneo dura muchisímo tiempo en finalizar, por lo tanto, podemos intuir que la contraseña está muy abajo en el diccionario (teniendo en cuenta que el rockyou.txt tiene alrededor de 14 millones de contraseñas).

<img width="1528" height="792" alt="hedgehog6" src="https://github.com/user-attachments/assets/4d2cee8d-afb8-4873-9af8-3906193e4bd5" />

Entonces procederemos a voltear el diccionario para que comienze de abajo hacía arriba, y eliminaremos los espacios, con los siguientes comandos:

<img width="900" height="328" alt="hedgehog7" src="https://github.com/user-attachments/assets/8a043ebf-75a7-4a57-8dee-8236d7bd30e7" />

Lanzamos nuevamente la fuerza bruta con hydra, encontrando de inmediato la contraseña, para así ganar acceso a la máquina víctima:

<img width="1520" height="512" alt="hedgehog8" src="https://github.com/user-attachments/assets/18a6210b-6675-4ea6-a4ca-15d186162f49" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina víctima, procederemos con la escalada de privilegios, daremos el comando "sudo -l" para ver si tenemos permisos a nivel de sudoers, y efectivamente podemos ejecutar cualquier binario con el usuario sonic, pivotamos al usuario sonic con el siguiente comando, luego ya como sonic, volveremos a ejecutar "sudo -l", encontrando que ahora como root tambien podemos ejecutar cualquier binario, daremos el mismo comando pero con usuario root y ¡máquina hackeada! 🔥 . .

<img width="1004" height="566" alt="hedgehog9" src="https://github.com/user-attachments/assets/70d97db7-6a6a-48ef-925c-861bd3194093" />

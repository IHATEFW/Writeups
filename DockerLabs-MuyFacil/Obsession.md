Octava máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina permite loguearnos en el servicio FTP con el usuario Anonymous, donde se roban 2 archivos .txt, en los cuales se exponen usuarios válidos . .

## OBSESSION

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1521" height="589" alt="obsession1" src="https://github.com/user-attachments/assets/38410c70-7b97-4d15-9901-ed6252ada843" />

## 🔎 ENUMERACIÓN

Como primer paso, procederemos a enumerar los puertos abiertos que pueda tener la máquina víctima, esto para saber que servicio comprometer de cara a ganar acceso a la máquina, lo realizaremos con la herramienta nmap, de la siguiente manera:

<img width="1521" height="781" alt="obsession2" src="https://github.com/user-attachments/assets/317f1032-fe99-4f21-8dd7-462f80eeb863" />

Ya efectuado el primer escaneo, nos arroja que existen los puertos 21, 22 y 80 abiertos, relacionados a los servicios FTP, SSH y HTTP, por lo tanto, procederemos a seguir enumerando con la herramienta nmap, esta vez indicandole que nos descubra la versión de dichos servicios y que nos arroje un conjunto básico de scripts de reconocimiento, lo lanzamos y nos muestra información importante, en este caso podemos darnos cuenta que el servicio FTP acepta loguearnos con el usuario Anonymous, y nos muestra ciertos archivos críticos, por lo tanto, lo primero que haremos es descargarnos esos archivos para ver que contiene:

<img width="1532" height="788" alt="obsession3" src="https://github.com/user-attachments/assets/8800367a-11f2-4b97-bdaa-7e31f9774bd3" />

## 💣 EXPLOTACIÓN

Nos logueamos por FTP con el usuario Anonymous y nos descargamos los archivos:

<img width="1526" height="792" alt="obsession4" src="https://github.com/user-attachments/assets/4c880771-cb17-456b-a643-ae17c252493f" />

Una vez ya tenemos los archivos .txt en nuestra máquina atacante, vemos que contienen la siguiente información:

<img width="1193" height="477" alt="obsession5" src="https://github.com/user-attachments/assets/befc6134-b5e9-4814-a68f-15af83eb91f8" />

Podemos darnos cuenta que posiblemente existan 2 usuarios válidos (Russosky y Gonza), en este punto procederemos a revisar la página web que está en el puerto 80, abrimos un navegador y colocamos la ip de la máquina víctima, nos muestra la siguiente página web, del supuesto Russoski, quien es entrenador personal e informático, tiene comentarios machistas sobre las mujeres, con mayor razón hackearemos su máquina 😈.

<img width="1521" height="788" alt="obsession6" src="https://github.com/user-attachments/assets/edf6dd49-1404-4191-a692-5ea8ed32b674" />

<img width="1524" height="788" alt="obsession7" src="https://github.com/user-attachments/assets/5477a453-43f1-401f-b39f-680274ab2b28" />

Exploraremos el código fuente con CTRL + U, y nos damos cuenta de un comentario que el mismo pone "Utilizando el mismo usuario para todos mis servicios, podré recordarlo fácilmente", por lo tanto, sabemos que el usuario Russoski es el indicado.

<img width="1520" height="424" alt="obsession8" src="https://github.com/user-attachments/assets/ae33722a-0204-47b1-b03a-5279ecec5798" />

Prodeceremos a ejecutar fuerza bruta de SSH con la herramienta Hydra de la siguiente manera, encontrando la contraseña de Russoski, ¡Ganamos acceso a la máquina víctima! 🔥

<img width="1518" height="625" alt="obsession9" src="https://github.com/user-attachments/assets/52357220-4c92-4c7c-8911-658c79adfd61" />

## 🔑 ESCALADA DE PRIVILEGIOS

Una vez dentro de la máquina víctima, tenemos 2 opciones para elevar privilegios a root, la primera es dando el comando "sudo -l", donde podemos ejecutar el binario /usr/bin/vim con privilegios de root, de la siguiente manera:

<img width="1161" height="268" alt="obsession10" src="https://github.com/user-attachments/assets/6ee1b8d9-d69d-4ea3-8837-e9ac8763aac7" />

Una vez dentro del editor vim, daremos el comando :!/bin/bash y seremos root

<img width="1534" height="789" alt="obsession12" src="https://github.com/user-attachments/assets/40bdfacb-9866-4c9b-8294-4432ffc66143" />

<img width="384" height="80" alt="obsession11" src="https://github.com/user-attachments/assets/f722cec7-1be5-4f25-8737-b875f86b0835" />

La otra opción es encontrar un archivo, en la ruta /var/www/html/important, que se llama .root-passwd.txt, en el cual existe un hash, que al decodearlo nos muestra la contraseña de root.

<img width="1042" height="224" alt="obsession13" src="https://github.com/user-attachments/assets/52c664d2-1290-4124-adab-00224e087c30" />

<img width="1496" height="432" alt="obsession14" src="https://github.com/user-attachments/assets/b47dd798-b752-4353-a186-c33f8b613ed4" />

Máquina hackeada ✨

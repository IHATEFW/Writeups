
# SHOWTIME

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="642" height="582" alt="show1" src="https://github.com/user-attachments/assets/243ea723-51cf-49b4-8c1a-fdeb49c8280e" />

## 🔎 ENUMERACIÓN

Como primer paso para compremeter esta máquina, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos de la máquina víctima, esto de la siguiente manera, una vez ejecutado el comando, podemos visualizar que existen dos puertos abiertos, el 22 y el 80, correspondientes a los servicios SSH y HTTP.

<img width="766" height="545" alt="show2" src="https://github.com/user-attachments/assets/5ac5c813-266c-4c81-907e-4fb3435881cd" />

Una vez ya sabemos los puertos que existen abiertos, procederemos a seguir enumerando con la herramienta nmap, pero esta vez con un escaneo más exhaustivo, le diremos a nmap que nos encuentre la versión de dichos servicios y además, que nos arroje un conjunto básico de scripts de reconomiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar que el titulo de la página web es "cs", procederemos a revisarla.

<img width="876" height="629" alt="show3" src="https://github.com/user-attachments/assets/b5b80feb-4108-4eba-af40-f3031a77765f" />

Es una web que hace referencia a una especie de "casino", donde tiene un panel de Login, vamos abrirlo.

<img width="1307" height="621" alt="show4" src="https://github.com/user-attachments/assets/2d1670c3-8b65-43a0-b816-f4ad2fe21bf5" />

Una vez dentro del panel de login, como no tenemos credenciales válidas, probaremos a realizar una inyección SQL, con el típico comando admin'OR 1=1;-- -

<img width="1313" height="611" alt="show5" src="https://github.com/user-attachments/assets/03deb10e-fca4-4603-a8dc-953dd00ff569" />

Vemos que estamos dentro, por lo tanto, es vulnerable a un SQLi, esta técnica permite validar como OK cualquier entrada en dicho panel, logrando acceso.

<img width="1159" height="541" alt="show6" src="https://github.com/user-attachments/assets/8a29a148-cf78-4343-8370-f52da024b51c" />

En este punto, procederemos a activar nuestro foxyproxy y abrir burpsuite en segundo plano, para así volver a lanzar esa petición y así poder ver como se está tramitando.

<img width="1330" height="580" alt="show7" src="https://github.com/user-attachments/assets/c8de1c3f-d590-42b7-ad92-98ed8545e054" />

<img width="1146" height="582" alt="show8" src="https://github.com/user-attachments/assets/b99bee5a-16d1-44ac-bdf8-a4ff6b0851da" />

Nos copiamos toda la petición para proceder a guardarla en un archivo .txt, de la siguiente manera:

<img width="721" height="461" alt="show9" src="https://github.com/user-attachments/assets/e4b6737f-91a9-4b65-8040-f1ce60509fc6" />

Ahora utilizaremos la herramienta sqlmap, para intentar enumerar la base de datos (las tablas), con el siguiente comando:

<img width="876" height="615" alt="show10" src="https://github.com/user-attachments/assets/56952f8c-1e16-44c1-85ae-dbe8cab49b67" />

<img width="876" height="615" alt="show11" src="https://github.com/user-attachments/assets/ce53d4de-b9bb-4acf-96d5-7707c13014da" />

Vemos que se enumeran las tablas, ahora seguiremos utilizando la herramienta sqlmap, pero esta vez para explotar y dumpearla para que nos arroje usuarios y contraseñas válidas.

<img width="876" height="615" alt="show12" src="https://github.com/user-attachments/assets/14737cb0-9923-428f-ac72-6ceadb6cb655" />

<img width="883" height="622" alt="show13" src="https://github.com/user-attachments/assets/49c9141b-c40c-4111-b350-3f50ae418b2b" />

Vemos que nos arrojó credenciales válidas, probaremos ingresar al anterior panel de login con los 3 usuarios, pero solo nos muestra información importante el usuario "joe", que es un panel de administración que nos permite ejecutar código python.

<img width="883" height="622" alt="show14" src="https://github.com/user-attachments/assets/c8f84454-4cad-4f6c-9561-1d2c4965d3db" />

Procederemos a inyectar código python para lanzarnos una reverse shell a nuestra máquina atacante.

<img width="883" height="622" alt="show15" src="https://github.com/user-attachments/assets/4ca63db1-65be-47ee-a115-6803a083d01f" />

Nos ponemos en escucha con la herramienta netcat por el puerto 443, lanzamos el comando y ¡Ganamos acceso a la máquina víctima!

<img width="743" height="372" alt="show16" src="https://github.com/user-attachments/assets/757b82ce-3540-49de-99be-6a410e9b4dc9" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```

Ya con la tty más estable, procederemos a leer el archivo /etc/passwd para ver si existen más usuarios dentro del sistema y vemos que existen los usuarios "joe" y "luciano".

<img width="734" height="573" alt="show17" src="https://github.com/user-attachments/assets/b3573d84-9630-4051-a463-ed6ad0c966a9" />

Nos dirigiremos al directorio /tmp y con ls -ltra vemos que existe un archivo llamado .hidden_text.txt, donde se exponen credenciales válidas pero en mayusculas.

<img width="603" height="631" alt="show18" src="https://github.com/user-attachments/assets/c5d65429-a29f-47e0-a02a-8de322c8a201" />

Procederemos a ponerlas en minusculas con el siguiente comando:

<img width="777" height="631" alt="show19" src="https://github.com/user-attachments/assets/6b6762b2-3138-4d37-8423-6b0e5d25d72b" />

Ahora vamos a realizar fuerza bruta con la herramienta Sudo_BruteForce del pinguino de Mario (Mario Fernández), la que nos permitirá saber cual es la contraseña válida para joe o luciano.

<img width="856" height="631" alt="show20" src="https://github.com/user-attachments/assets/759f23e3-eeff-4bc2-9965-499d4ca5f864" />

Descargaremos el script .sh, lo pondremos en formato "Raw" y nos traeremos la herramienta con el siguiente comando

<img width="856" height="631" alt="show21" src="https://github.com/user-attachments/assets/395dc7f9-32ce-414d-8a70-dbf8f901ae49" />

Le daremos permisos de ejecución con chmod 777.

<img width="1017" height="631" alt="show22" src="https://github.com/user-attachments/assets/0e6b4ee0-ecfb-4a5e-a8b0-7a365a796291" />

La ejecutamos con el siguiente comando:

Y encontramos la contraseña válida para el usuario joe, entramos y damos el comando sudo -l para ver si tenemos permisos a nivel de sudoers y vemos que efectivamente podemos ejecutar el binario /bin/posh como el usuario luciano.

<img width="535" height="217" alt="show23" src="https://github.com/user-attachments/assets/c200c6b5-6a7b-449f-a00b-8e35186f59e5" />

Ejecutamos sudo -u luciano posh y ya somos luciano, hacemos lo mismo y vemos que podemos ejecutar un script que se encuentra en el directorio /home/luciano

<img width="1026" height="338" alt="show24" src="https://github.com/user-attachments/assets/93cc1fe2-6080-4af5-bd3b-01e97200d489" />

<img width="1024" height="146" alt="show25" src="https://github.com/user-attachments/assets/5749c507-1246-490a-ad62-d25df246a159" />

<img width="588" height="425" alt="show26" src="https://github.com/user-attachments/assets/fef02d0f-88fd-45f0-bd2d-0f7fc4461126" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

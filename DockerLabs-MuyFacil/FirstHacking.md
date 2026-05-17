Segunda máquina de la plataforma dockerlabs.es, de la categoría "Muy Fácil", comenzaremos a tocar otro servicio, llamado FTP (File Transfer Protocol), que mal configurado, puede ser muy peligroso . .

# FIRSTHACKING

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="905" height="329" alt="firsthacking1" src="https://github.com/user-attachments/assets/655bdf60-aecf-44ea-b56d-fc668cb4aa11" />

## 🔎 ENUMERACIÓN

Procederemos a enumerar los puertos abiertos con la herramienta nmap, con el siguiente comando:

<img width="1501" height="785" alt="firsthacking2" src="https://github.com/user-attachments/assets/58fc2bf3-a16a-4c43-9f5d-eae895be89c3" />

Vemos que existe el puerto 21 expuesto, correspondiente al servicio FTP, ahora seguiremos enumerando nuevamente con la herramienta nmap, indicandole que nos descubra la versión del servicio FTP y que nos arroje un conjunto básico de script de reconocimiento, esto para tener más información de cara a la explotación:

<img width="1028" height="785" alt="firsthacking3" src="https://github.com/user-attachments/assets/7f02f4e6-a02d-4828-bd62-65f02d88dca2" />

Nos arroja la versión de ftp llamada vsftpd 2.3.4, la cual es una versión vulnerable, que crea una puerta trasera (backdoor) para lograr acceso a la máquina víctima, por lo tanto, ejecutaremos el comando searchsploit para buscar el exploit correcto y ejecutarlo, searchsploit es el comando que interactua con la base de datos de exploit-db pero a nivel de terminal.

<img width="1520" height="309" alt="firsthacking4" src="https://github.com/user-attachments/assets/01e5da69-21bc-4881-aa4e-4fffd4a229b3" />

## 💣 EXPLOTACIÓN

Nos muestra un exploit desarrollando en Ruby, que se puede lanzar con metasploit, por lo tanto, procederemos a abrir metasploit con el comando msfconsole para utilizarlo:

<img width="1498" height="785" alt="firsthacking5" src="https://github.com/user-attachments/assets/50e10e2b-f0f8-435e-9aa5-4972f0a6e7b5" />

Lo buscamos, lo encontramos con la numeración "0", lo usamos, lo configuramos y lo corremos con run:

<img width="1498" height="785" alt="firsthacking5" src="https://github.com/user-attachments/assets/27dbad6b-c16d-45b1-a1c6-d872396498b6" />

<img width="1510" height="786" alt="firsthacking6" src="https://github.com/user-attachments/assets/47c63ea7-cae3-445b-ba35-fac1c368333a" />

Finalmente ya somos usuario root, así de fácil, máquina hackeada 🔥.

Por último para que se vea más limpio, podemos lanzarnos una reverse shell a nuestra máquina atacante para salir de metasploit.

<img width="1530" height="785" alt="firsthacking7" src="https://github.com/user-attachments/assets/7afc9562-97a9-4714-bac8-87e08b16e884" />

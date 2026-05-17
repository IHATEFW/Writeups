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


## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

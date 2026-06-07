La máquina Candy de la plataforma DockerLabs.es, es de dificultad "Fácil", y nos muestra/enseña como un gestor de contenido (CMS), llamado Joomla, puede darnos acceso a la máquina víctima logrando editar uno de sus archivos .php de configuración . .   

# CANDY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1163" height="530" alt="candy1" src="https://github.com/user-attachments/assets/e85b87eb-38e8-477c-b03f-2545e25f0fae" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que solamente exíste el puerto 80 expuesto, correspondiente al servicio HTTP.

<img width="1148" height="516" alt="candy2" src="https://github.com/user-attachments/assets/0f7b80a8-0606-421f-ae93-e67e40b020f4" />
<img width="1060" height="637" alt="candy3" src="https://github.com/user-attachments/assets/238417b3-bdfa-4e23-85fd-3c766dee0307" />


## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

Séptima máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", esta máquina expone en la página web del puerto 80, un usuario válido, del cual se aprovecha para ganar acceso a la máquina víctima . .

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1305" height="342" alt="vacaciones1" src="https://github.com/user-attachments/assets/1aa24a21-4486-4239-9f3b-fc1a3a127345" />

## 🔎 ENUMERACIÓN

Comenzaremos a resolver esta máquina utilizando la herramienta nmap para enumerar los puertos abiertos, esto nos permitirá escoger alguna vía de ingreso a nuestra máquina víctima, lo realizaremos con el siguiente comando, una vez ejecutado, podemos visualizar que encontramos los puertos 22 y 80 abiertos, relacionados a los servicios SSH y HTTP:

<img width="1510" height="780" alt="vacaciones2" src="https://github.com/user-attachments/assets/bf806d9a-e180-4991-afb8-a3ca504ac0ba" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

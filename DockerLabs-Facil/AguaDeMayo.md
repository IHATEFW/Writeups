
# AGUADEMAYO

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="648" height="428" alt="agua1" src="https://github.com/user-attachments/assets/2399748d-75ba-4fbd-89ba-94eff92b6894" />

## 🔎 ENUMERACIÓN

El primer paso que deberemos realizar para comprometer esta máquina, es comenzar con la fase de enumeración, esto lo realizaremos escaneando puertos con la herramienta nmap, esto con el fin de identificar posibles puertos abiertos/expuestos que tenga la máquina víctima, lo realizaremos con el siguiente comando, una vez ejecutado, podemos visualizar que existen dos puertos abiertos, el 22 y el 80, relacionados a los servicios SSH y HTTP.

<img width="781" height="558" alt="agua2" src="https://github.com/user-attachments/assets/e019f889-0741-4296-8e08-7b61bdea9a7e" />

Una vez ya tenemos los puertos abiertos identificados, procederemos a seguir enumerando con nmap, pero esta vez indicandole que nos encuentre las versiones de esos servicios y que nos arroje un conjunto básico de scripts de reconomiento, una vez ejecutado, podemos visualizar que la web detrás del puerto 80 es la típica de Apache2 que viene por defecto.

<img width="847" height="629" alt="agua3" src="https://github.com/user-attachments/assets/500c570b-1821-4c92-a750-3bf2e2711bbf" />

Le echamos un vistazo y ha simple vista no tiene nada importante.

<img width="1136" height="628" alt="agua4" src="https://github.com/user-attachments/assets/0b7396a1-1695-4c2e-9001-5ec752de1ea7" />

Pero le damos un CTRL + U y podemos visualizar al final de la web que nos entrega una especie de mensaje encriptado/caracteres raros, esto está encriptado en el lenguaje de programación "Brainfuck".

<img width="1236" height="630" alt="agua5" src="https://github.com/user-attachments/assets/a13ebbf8-2443-45e1-8fd0-f4623d0e4654" />

Lo copiamos y procedemos abrir alguna decoder de brainfuck que nos desencripte dicho mensaje.

<img width="913" height="597" alt="agua6" src="https://github.com/user-attachments/assets/52546348-8503-4c49-bca2-f63bf03d0ecf" />

<img width="913" height="597" alt="agua7" src="https://github.com/user-attachments/assets/35e20e8e-6354-4b2b-9f9f-24ae30a434e9" />

<img width="860" height="412" alt="agua8" src="https://github.com/user-attachments/assets/69627f7b-da96-4b26-8321-5b620c784d47" />

<img width="930" height="286" alt="agua9" src="https://github.com/user-attachments/assets/06281975-75f2-45aa-aa04-cb00e10fc9b0" />

<img width="934" height="105" alt="agua10" src="https://github.com/user-attachments/assets/3387bdb5-6d4e-4a89-8c5b-ea0396db3836" />

<img width="929" height="421" alt="agua11" src="https://github.com/user-attachments/assets/5dfb7dbc-0183-40fb-a7e3-55ed3ee6598d" />

<img width="789" height="251" alt="agua12" src="https://github.com/user-attachments/assets/9ae09195-7faf-402d-ad63-b49d328e8437" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

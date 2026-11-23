
# ANALYST

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1848" height="874" alt="ana1" src="https://github.com/user-attachments/assets/929a221c-ce36-476e-ae6b-d53c6fc9948d" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1819" height="656" alt="ana2" src="https://github.com/user-attachments/assets/e5e23442-600a-40ae-b69f-e5ff69b02304" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "El caso de Pinguinito / Lab Blue Team".

<img width="1808" height="786" alt="ana3" src="https://github.com/user-attachments/assets/3a7590c2-681e-4b30-9d27-8f2c2f0eea29" />

<img width="1878" height="909" alt="ana4" src="https://github.com/user-attachments/assets/1314e7e3-f4fa-4a70-858c-607806d94289" />

<img width="1878" height="909" alt="ana5" src="https://github.com/user-attachments/assets/5d9f994e-1a5e-4d64-b160-cbb65ad932db" />

<img width="1191" height="225" alt="ana6" src="https://github.com/user-attachments/assets/60859b1f-7256-42d7-a315-330ff528254c" />

<img width="1882" height="911" alt="ana7" src="https://github.com/user-attachments/assets/8440b2a4-0744-46e1-9d2c-0816957ac367" />

<img width="1882" height="911" alt="ana8" src="https://github.com/user-attachments/assets/1be2b28c-61d8-483a-9e98-003a37c20f68" />

<img width="1882" height="911" alt="ana9" src="https://github.com/user-attachments/assets/a454bd1c-c14a-485c-be40-be8d4f95501e" />

<img width="1882" height="911" alt="ana10" src="https://github.com/user-attachments/assets/8ef8d549-c287-48b4-b5ec-b6fe4455f17e" />

<img width="1882" height="911" alt="ana11" src="https://github.com/user-attachments/assets/f4bde241-c5a4-4b96-9fc7-032262984855" />

<img width="1882" height="911" alt="ana12" src="https://github.com/user-attachments/assets/916f3c7c-81bb-463d-b564-b9bb45bebf0e" />

<img width="1882" height="911" alt="ana13" src="https://github.com/user-attachments/assets/e739722a-f6fd-4e96-864c-1267db634c75" />

<img width="1882" height="911" alt="ana14" src="https://github.com/user-attachments/assets/78732cf6-f4a0-4cf0-930e-a2ce0bdecbcc" />

<img width="1882" height="911" alt="ana15" src="https://github.com/user-attachments/assets/06a4d601-f2a4-4bff-8ee3-bfe68befcb9e" />

<img width="1882" height="911" alt="ana16" src="https://github.com/user-attachments/assets/44f93bb1-3fc0-483b-a311-6f0ec3784a6f" />

<img width="1882" height="911" alt="ana17" src="https://github.com/user-attachments/assets/fe780225-5de7-41ca-8e60-befa0813ac77" />

<img width="1882" height="911" alt="ana18" src="https://github.com/user-attachments/assets/7741fd43-f0c6-4f5b-baa6-08a0af94d130" />

<img width="1882" height="911" alt="ana19" src="https://github.com/user-attachments/assets/e97c7d8c-ea58-4b32-92a2-27dbc659a5ff" />

<img width="1882" height="911" alt="ana20" src="https://github.com/user-attachments/assets/39add498-ae7d-4352-848b-56d33c739f63" />

<img width="1882" height="911" alt="ana21" src="https://github.com/user-attachments/assets/d20e8822-2047-4f22-a0fe-4fc3b00711e9" />

<img width="1882" height="911" alt="ana22" src="https://github.com/user-attachments/assets/90cdc864-80f4-46eb-91e6-29fee9bc0392" />

<img width="1600" height="900" alt="ana23" src="https://github.com/user-attachments/assets/0b8c4294-a31c-4c9f-85ce-2cf982ce4e08" />

<img width="1600" height="900" alt="ana24" src="https://github.com/user-attachments/assets/bdd02752-a27d-4d3d-abb6-f73cbf108498" />

<img width="1600" height="900" alt="ana25" src="https://github.com/user-attachments/assets/2d988167-c818-4c90-8f1a-b38616e92077" />

<img width="1600" height="900" alt="ana26" src="https://github.com/user-attachments/assets/6b81543a-7d4b-4e13-8e15-25ae0f4954df" />

<img width="1600" height="900" alt="ana27" src="https://github.com/user-attachments/assets/16af9264-7890-4818-9c76-17a50588c244" />

<img width="1600" height="900" alt="ana28" src="https://github.com/user-attachments/assets/1b04b64e-c03d-4671-8650-113b3529565e" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

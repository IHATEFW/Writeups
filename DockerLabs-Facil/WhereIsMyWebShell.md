
# WHEREISMYWEBSHELL

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="957" height="299" alt="where1" src="https://github.com/user-attachments/assets/4e6beef8-0646-4009-99d8-dba5fc3c65e7" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que solo existe el puerto 80 abierto, correspondiente al servicio HTTP.

<img width="1255" height="631" alt="where2" src="https://github.com/user-attachments/assets/66364a9e-7dc1-46aa-8694-a9f48ecde1c7" />

Una vez ya tenemos el puerto abierto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio HTTP, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "Academia de Inglés".

<img width="1036" height="577" alt="where3" src="https://github.com/user-attachments/assets/38fb554f-975e-4a87-8ac1-a56c1de6567d" />

Lanzamos el comando Whatweb para ver las tecnologías que se están empleando por detrás de la página web.

<img width="1332" height="252" alt="where4" src="https://github.com/user-attachments/assets/d44e1761-58df-46b9-af55-3b7858e6a3a8" />

Revisamos la web y efectivamente hace referencia a una Academia de Inglés.

<img width="1226" height="627" alt="where5" src="https://github.com/user-attachments/assets/f39ccf5d-dd2e-45b3-ae52-4bf1991ba097" />

Seguimos scrolleando hasta el final y nos entregan una pista, indicando que guardaron un secreto en el directorio /tmp

<img width="1242" height="303" alt="where6" src="https://github.com/user-attachments/assets/6840ec59-eef1-451f-abb4-558a08313ae3" />

En este punto

<img width="1336" height="617" alt="where7" src="https://github.com/user-attachments/assets/036ed4fa-79cc-47e4-96b7-548c66c6cc7d" />

<img width="1205" height="309" alt="where8" src="https://github.com/user-attachments/assets/391f51eb-0dd0-4832-a337-2c7e3e2e7d59" />

<img width="1340" height="617" alt="where9" src="https://github.com/user-attachments/assets/ee500c72-fa05-4f1d-946c-7d7fa7493ed4" />

<img width="657" height="212" alt="where10" src="https://github.com/user-attachments/assets/84a33102-64d3-458d-8a28-495c3f90a0c4" />

<img width="549" height="272" alt="where11" src="https://github.com/user-attachments/assets/fa6773df-1de4-4609-a064-2576e638e5ba" />

<img width="880" height="188" alt="where12" src="https://github.com/user-attachments/assets/e5403c82-5dc9-4905-9cf5-cb306329db51" />

<img width="710" height="389" alt="where13" src="https://github.com/user-attachments/assets/a649aad4-3594-4d10-bc96-bec6309ec9cc" />

<img width="482" height="487" alt="where14" src="https://github.com/user-attachments/assets/00fa3950-0fe2-4a66-8d77-0f7627d1a79b" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

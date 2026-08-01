La máquina Mirame de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos muestra como un panel de login de autenticación es vulnerable a SQLi, logrando también ocupar la herramienta SQLmap y dumpear toda la base de datos, extrayendo información crítica para ganar acceso a la máquina víctima, esto también utilizando un poco de esteganografía, una vez dentro de la máquina víctima logramos pivotar a root por un binario con permisos SUID . .

# MIRAME

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="680" height="598" alt="mirame1" src="https://github.com/user-attachments/assets/8f4ef225-e41a-4b4e-8554-0f4320212c51" />

## 🔎 ENUMERACIÓN

Cuando ya tengamos la ip de la máquina víctima, se procederá a realizar un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos existentes abiertos/expuestos que tenga la máquina, esto con el siguiente comando, una vez ejecutado, podemos visualizar que tenemos 2 puertos abiertos, el puerto 22 correspondiente al servicio SSH y el puerto 80 correspondiente a HTTP.

<img width="807" height="550" alt="mirame2" src="https://github.com/user-attachments/assets/1e751819-87a8-4ff7-872f-0cf81ed1751c" />

Ya cuando tengamos los puertos abiertos identificados, seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que al parecer la web que hay por detrás del puerto 80 tiene un panel de login de autenticación según nos arrojó el escaneo, vamos a revisarla.

<img width="807" height="550" alt="mirame3" src="https://github.com/user-attachments/assets/5ca54f69-b2df-4c9c-9da1-f300a07d1824" />

Efectivamente tenemos un login de autenticación, pero no tenemos ninguna credencial en este punto.

<img width="1243" height="614" alt="mirame4" src="https://github.com/user-attachments/assets/e35f9833-d36d-4c1a-8b11-37f753bad1e1" />

Por lo tanto, procederemos a probar una SQLi (Inyección SQL), esto de la siguinete manera:

<img width="1243" height="614" alt="mirame5" src="https://github.com/user-attachments/assets/aea4acec-0f28-459b-a4d2-71a1cf274935" />

## 💣 EXPLOTACIÓN

Vemos que es vulnerable a una SQLi, ¡Logramos acceso al panel! 🔥

<img width="1243" height="614" alt="mirame6" src="https://github.com/user-attachments/assets/92c4311b-1899-4017-a5eb-36663e2149d2" />

Vemos que es una especie de consulta de clima por ciudad, probamos con la ciudad de Santiago y nos arroja una temperatura.

<img width="1243" height="614" alt="mirame7" src="https://github.com/user-attachments/assets/a7ab978e-81dc-40c3-92fd-33032f3b2b76" />

En este punto, procederemos a abrirnos Burpsuite, para interceptar la petición de la SQLi.

<img width="1165" height="591" alt="mirame8" src="https://github.com/user-attachments/assets/ae4f39bc-a8d1-4ab7-b6d1-37e07d78eb5a" />

La volvemos a lanzar, pero en el campo de usuario ponemos admin y en la contraseña también.

<img width="1134" height="620" alt="mirame9" src="https://github.com/user-attachments/assets/d1e86d41-1fee-410c-a779-8c56e2f5b3d8" />

Nos guardamos toda dicha petición en un archivo llamado request.txt

<img width="1105" height="549" alt="mirame10" src="https://github.com/user-attachments/assets/a95d7f2d-bac4-453b-a2f5-003fd006d274" />

Y lo ocuparemos para enumerar las bases de datos con SQLmap, esto de la siguiente manera:

<img width="1144" height="624" alt="mirame11" src="https://github.com/user-attachments/assets/dd7827bf-7612-4184-b1bc-e5aecfb7683c" />

Una vez ejecutado el escaneo, podemos visualizar que nos enumeró 2 bases de datos.

<img width="1144" height="624" alt="mirame12" src="https://github.com/user-attachments/assets/b65ad856-97a6-4501-89d4-4436544c1a1c" />

Ahora lanzaremos nuevamente el escaneo pero esta vez con el parametro -dump para dumpear todas las bases de datos y extraer información importante.

<img width="1144" height="624" alt="mirame13" src="https://github.com/user-attachments/assets/3d92e03e-49cc-47f3-8326-ca24d34e7957" />

Y efectivamente conseguimos nuestro propósito, pero nos llama la atención donde dice "directoriotravieso", hace referencia a un posible directorio que no hemos visto.

<img width="1144" height="624" alt="mirame14" src="https://github.com/user-attachments/assets/277e9665-4db5-4c4a-a8b7-cd5fe4aa6d42" />

Vamos a probarlo en la url luego de la /, y conseguimos acceder a una imagen .jpg, llamada miramebien.

<img width="730" height="438" alt="mirame15" src="https://github.com/user-attachments/assets/de7386cb-9e77-427c-8480-3580d5c72bbb" />

<img width="1062" height="568" alt="mirame16" src="https://github.com/user-attachments/assets/f91cdcd3-7878-4d92-9938-807f526cee2b" />

La descargamos en nuestra máquina atacante para probar posible esteganografía que pueda tener la imagen, como metadatos importantes o credenciales dentro de ella.

<img width="1224" height="568" alt="mirame17" src="https://github.com/user-attachments/assets/8f6403d0-0cc6-422a-b765-94bf06e002d2" />

Procedemos a utilizar distintas herramientas, como exiftool, pero no nos muestra nada importante.

<img width="750" height="633" alt="mirame18" src="https://github.com/user-attachments/assets/946698af-b9a7-4fb3-939c-a589e93297e3" />

Ahora probamos con la herramienta stegcracker y nos encontró la password "chocolate".

<img width="790" height="501" alt="mirame19" src="https://github.com/user-attachments/assets/f174c98f-189c-4ab3-b7c3-1a40b7232aa8" />

Ahora utilizaremos la herramienta steghide para extraer información oculta en la imagen, nos pide una contraseña de salvoconducto, le damos la "chocolate" y nos extrae un archivo llamado ocultito.zip

<img width="790" height="402" alt="mirame20" src="https://github.com/user-attachments/assets/714af836-733e-4cb8-8278-5f4a2bc7448f" />

El cual nos podemos descomprimir porque nos pide otra contraseña, en este punto utilizaremos la herramienta fcrackzip para pasarle el diccionario rockyou.txt y nos encontró la contraseña "stupid1", logramos descomprimir el archivo .zip y encontramos la password de un usuario válido del sistema, carlos:carlitos. 

<img width="1058" height="512" alt="mirame21" src="https://github.com/user-attachments/assets/a361aa09-9a35-49c3-8bd4-7e1e84685979" />

Accedemos vía SSH a la máquina víctima con las credenciales que encontramos, y ¡efectivamente logramos acceso!.

<img width="937" height="505" alt="mirame22" src="https://github.com/user-attachments/assets/aa213ebd-e56d-4607-8596-02c7fedb3f64" />

## 🔑 ESCALADA DE PRIVILEGIOS

Arrojamos el siguiente comando para ver si existen binarios con permisos SUID para poder ejecutar y encontramos /usr/bin/find, que sabemos que es crítico.

<img width="937" height="505" alt="mirame23" src="https://github.com/user-attachments/assets/1a7a71ec-980d-431e-b739-efa9120cffef" />

Nos dirigiremos a nuestra biblia gtfobins.org y filtramos con find, en el apartado de SUID nos copiamos el primer comando.

<img width="1144" height="584" alt="mirame24" src="https://github.com/user-attachments/assets/7a5a2446-3b86-4a65-8c6f-a0c75633ad38" />

Lo arrojamos en la máquina victima y finalmente pivotamos al usuario root, máquina hackeada 🔥. .

<img width="651" height="409" alt="mirame25" src="https://github.com/user-attachments/assets/e268c9b8-d825-4686-8263-a0fc9b68e316" />

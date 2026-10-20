La máquina HiddenCat de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como explotar un servidor web Tomcat en su versión 9.0.30 vulnerable a una lectura de archivos arbitraria, dicha vulnerabilidad se le asigno el CVE-2020-1938, encontrando un usuario válido del sistema para posteriormente encontrar su contraseña con una ataque de fuerza bruta SSH con hydra, luego dentro de la máquina víctima logramos pivotar a root por un binario con permisos SUID mal configurado . .

# HIDDENCAT

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1133" height="477" alt="hi1" src="https://github.com/user-attachments/assets/8536f5d6-ebf8-42e5-897e-0523d923662f" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22, 8009 y 8080, correspondientes a los servicios SSH, AJP y HTTP.

<img width="1513" height="636" alt="hi2" src="https://github.com/user-attachments/assets/350a0a48-e28c-44ac-bdb1-e696090de5ec" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar que detrás del puerto 8080 se está empleando un servidor web llamado Tomcat, en su versión 9.0.30

<img width="1594" height="909" alt="hi3" src="https://github.com/user-attachments/assets/eb9b58b2-e684-46e0-96ac-ff109611722a" />

## 💣 EXPLOTACIÓN

Vamos a revisar la web del puerto 8080 y vemos que nos muestra la típica web por defecto de Tomcat.

<img width="1594" height="909" alt="hi4" src="https://github.com/user-attachments/assets/c5866765-3c91-44db-8b8f-58e5b58a04db" />

En la siguiente ruta logramos encontrar credenciales por defecto para loguearnos tomcat:s3cret, pero tenemos un código 403 Access Denied.

<img width="1487" height="691" alt="hi5" src="https://github.com/user-attachments/assets/11403da5-39ce-4296-89e0-447cba6a73d3" />

Nos vamos a la web y filtramos por dicha versión que encontramos y vemos que es vulnerable a una lectura de archivos arbitraria, dicha vulnerabilidad se llamó Ghostcat y se le asigno el CVE-2020-1938.

<img width="1487" height="691" alt="hi6" src="https://github.com/user-attachments/assets/9aeec615-10bf-4efe-adf5-f6ac9b6d6f09" />

Volvemos a la terminal y buscamos en la base de datos de exploitdb filtrando por Ghostcat, nos arroja 2 scripts que podemos ocupar, un .py y un .rb

<img width="1863" height="401" alt="hi7" src="https://github.com/user-attachments/assets/36e11af5-1008-4481-a644-4527a489d74e" />

En este punto, decidimos escoger el siguiente repositorio de Github para clonarlo y traernos el siguiente script.

<img width="1897" height="782" alt="hi8" src="https://github.com/user-attachments/assets/e4e48b09-9c1f-4d05-859b-4abcd9e02197" />

<img width="1468" height="864" alt="hi9" src="https://github.com/user-attachments/assets/b9d815f6-7ac3-47c8-94ec-f5be8eaf3424" />

Le damos todos los permisos de ejecución y lo lanzamos según las instrucciones del archivo .md del repo, logrando efectuar la lectura de archivos arbitraria y encontrar un usuario válido del sistema, como es Jerry.

<img width="1463" height="892" alt="hi10" src="https://github.com/user-attachments/assets/db16b33a-d23c-43f0-bd40-8eb5bdbb8269" />

Realizaremos un ataque de fuerza bruta por SSH para intentar encontrar la contraseña de SSH de Jerry, y efectivamente, encontramos la password.

<img width="1898" height="449" alt="hi11" src="https://github.com/user-attachments/assets/6fc6d51a-0fb9-469b-a67f-fc4d8b6d4497" />

¡Ganamos acceso a la máquina víctima!

<img width="1174" height="661" alt="hi12" src="https://github.com/user-attachments/assets/5e632658-15fc-41db-8e80-2991080b2488" />

## 🔑 ESCALADA DE PRIVILEGIOS

Daremos el siguiente comando para buscar desde la raíz binarios con permisos SUID y encontramos varios, como perl y python.

<img width="653" height="590" alt="hi13" src="https://github.com/user-attachments/assets/dace72a6-d9e9-4164-b654-2bf172b4d416" />

Nos vamos a la web gtfobins.org y filtramos por Python y nos copiamos el primer comando que aparezca en el apartado de SUID.

<img width="1586" height="866" alt="hi14" src="https://github.com/user-attachments/assets/710a7862-fcfe-4f99-b3c0-8edca53158f1" />

Lo lanzamos de la siguiente manera y logramos pivotar al usuario root, ¡máquina hackeada!

<img width="1031" height="872" alt="hi15" src="https://github.com/user-attachments/assets/9558dbd4-3a2d-4ff8-856b-3a5f8d10154e" />

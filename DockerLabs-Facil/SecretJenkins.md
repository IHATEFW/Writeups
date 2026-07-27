
# SECRETJENKINS

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="690" height="418" alt="secret1" src="https://github.com/user-attachments/assets/6ae7655b-b110-429b-88ba-fd7aecb60df3" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, realizaremos un escaneo con la herramienta nmap, para que nos arroje todos los puertos abiertos existentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen los puertos 22 y 8080 abiertos, relacionados a SSH y un posible HTTP.

<img width="768" height="552" alt="secret2" src="https://github.com/user-attachments/assets/0a1d3431-3329-4ed0-a82d-97d19ff095e3" />

Ya sabemos que está el puerto 80 abierto, ahora seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dichos servicios SSH y HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos un header "Jetty 10.0.18", el cual posiblemente corresponde a un Jenkins.

<img width="837" height="640" alt="secret3" src="https://github.com/user-attachments/assets/ee6d278d-f209-4aa9-9682-8192f0a77508" />

Revisamos la web que corre en el puerto 8080 y efectivamente vemos que es un Jenkins, con el tipico panel de autenticación.

<img width="1203" height="640" alt="secret4" src="https://github.com/user-attachments/assets/8a4db74a-50f3-48ce-b1c0-225cd6b91812" />

En este punto, nuestra meta es verificar que versión corresponde a dicho Jenkins, para eso, lanzaremos el comando whatweb en nuestra terminal, logrando identificar que la versión es la 2.441.

<img width="1107" height="277" alt="secret5" src="https://github.com/user-attachments/assets/70a8f264-8196-4144-b998-6b4747b4203d" />

Ahora buscamos en la base de datos de exploit-db por dicha versión, pero no encontramos nada.

<img width="1107" height="277" alt="secret6" src="https://github.com/user-attachments/assets/061fa36d-b128-4a3c-a9dc-dceb28183343" />

Es allí cuando googleamos y nos damos cuenta que dicha versión si es vulnerable a un Arbitrary Read File, por lo tanto, encontramos este repositorio en Github, con un script de python que nos permitirá leer archivos dentro de la máquina víctima.

<img width="1234" height="634" alt="secret7" src="https://github.com/user-attachments/assets/be5db958-2328-40c6-a06e-b6e029602c4f" />

Clonamos dicho repositorio, y ejecutamos el script de la siguiente manera, y vemos que efectivamente nos muestra el archivo /etc/passwd, donde se exponen 2 usuarios "bobby" y "pinguinito".

<img width="1278" height="634" alt="secret8" src="https://github.com/user-attachments/assets/7d80d433-e86b-4416-8244-94076ca4310b" />

Como ya tenemos 2 usuarios válidos del sistema, vamos a ejecutar fuerza bruta de SSH con la herramienta Hydra, recordar que tambien teníamos el puerto 22 abierto, logrando encontrar la contraseña del usuario bobby, nos logueamos por ssh y ¡Ganamos acceso a la máquina víctima!

<img width="1278" height="634" alt="secret9" src="https://github.com/user-attachments/assets/0c94ee21-842e-4a76-befb-802fbfd74e0c" />

Ya dentro de la máquina como el usuario bobby

<img width="929" height="300" alt="secret10" src="https://github.com/user-attachments/assets/9259384c-0455-42b2-824c-a7bf4411bc65" />

<img width="1142" height="538" alt="secret11" src="https://github.com/user-attachments/assets/cfec8157-897d-4106-9edf-9683e49d1bb7" />

<img width="734" height="95" alt="secret12" src="https://github.com/user-attachments/assets/08ac78bf-8a79-4697-8f0b-d08e3dbef9d7" />

<img width="618" height="508" alt="secret13" src="https://github.com/user-attachments/assets/d4e83882-b25b-4446-b687-ac1d77988df6" />

<img width="668" height="486" alt="secret14" src="https://github.com/user-attachments/assets/d408be41-0013-4832-87b0-b833557eac8a" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

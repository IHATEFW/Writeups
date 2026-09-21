
# INFLUENCERHATE

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1236" height="867" alt="influ1" src="https://github.com/user-attachments/assets/e5a7a437-c6cf-4d04-b86a-cab6aadab7b9" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos 22 y 80 abiertos, relacionados a los servicios SSH y HTTP.

<img width="1511" height="615" alt="influ2" src="https://github.com/user-attachments/assets/23cbd1fb-5c12-49af-9efe-b093d602a200" />

Una vez ya tenemos los puertos abiertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar un http-auth indicando código 401 de unauthorized.

<img width="1335" height="912" alt="influ3" src="https://github.com/user-attachments/assets/b7fe59b6-6f47-4376-a1e0-567de8b39641" />

Arrojamos el comando whatweb para ver las tecnologías que corren por detrás y nos indica lo mismo.

<img width="1885" height="287" alt="influ4" src="https://github.com/user-attachments/assets/ec022262-a06c-4fee-8589-a32cfff27545" />

En este punto, procederemos a revisar la web, la cual nos muestra un campo de login, no tenemos credenciales válidas, probamos las típicas, inclusive probamos SQLi, pero no tenemos éxito.

<img width="1266" height="565" alt="influ5" src="https://github.com/user-attachments/assets/bc464814-a909-45c6-8c9a-5f088f1a7754" />

Abrimos Burpsuite para ver como se está tramitando la petición.

<img width="1540" height="915" alt="influ6" src="https://github.com/user-attachments/assets/daf65465-fb17-46f2-a317-eb0c6d504b82" />

Ahora procederemos a realizar un ataque de fuerza bruta de SSH con la herramienta hydra, para que nos encuentre el posible usuario válido con su password, esto lo haremos con un diccionario de credenciales por defecto, cambien le especificamos el puerto 80 y el método de la petición que es get, también le indicamos que busque desde la raíz /, una vez finaliza el escaneo nos encuentra las credenciales válidas.

<img width="1883" height="313" alt="influ7" src="https://github.com/user-attachments/assets/148fae9a-9173-4eb5-87a0-3da294cc2429" />

Como ya tenemos las credenciales válidas, realizaremos un ataque de fuerza bruta de directorios con la herramienta Gobuster, adjuntandole las credenciales que encontramos, una vez finaliza el escaneo no encuentra un /login.php

<img width="1885" height="749" alt="influ8" src="https://github.com/user-attachments/assets/d9582a3a-bed7-4564-958d-e344e2af037d" />

Convertiremos las credenciales que encontramos en base64

<img width="861" height="129" alt="influ9" src="https://github.com/user-attachments/assets/da11eb86-ad10-40b6-9d6f-d89ba9ecff17" />

Realizamos un fuzzing en el /login.php con las credenciales que encontramos y probando encontrar la posible password del usuario admin, una vez finaliza el escaneo nos encuentra la password admin, ahora si procederemos a revisar todas las webs.

<img width="1055" height="670" alt="influ10" src="https://github.com/user-attachments/assets/0c416200-32cf-4212-9866-e12a330e1d1f" />

<img width="1520" height="803" alt="influ11" src="https://github.com/user-attachments/assets/7237b0e3-9bf3-4cd4-9610-86d457d6885d" />

<img width="1440" height="901" alt="influ12" src="https://github.com/user-attachments/assets/cba040af-872b-46db-b7b2-79cdab94b2e1" />

<img width="664" height="339" alt="influ13" src="https://github.com/user-attachments/assets/b1c39ec5-6bd0-4b6b-a11e-45d006ed394d" />

<img width="1889" height="418" alt="influ14" src="https://github.com/user-attachments/assets/b37bda68-e693-482b-adcc-ac9c5f09a639" />

<img width="1181" height="454" alt="influ15" src="https://github.com/user-attachments/assets/84f24087-efaf-4040-9045-401acfdfc856" />

<img width="1056" height="863" alt="influ16" src="https://github.com/user-attachments/assets/ff8f27f1-ad93-483d-87c1-6b52c6ddfe20" />

<img width="1412" height="821" alt="influ17" src="https://github.com/user-attachments/assets/2de1e651-4c57-4689-b934-a2d032bd03d2" />

<img width="607" height="546" alt="influ18" src="https://github.com/user-attachments/assets/1558b3b7-aeed-4e6f-a599-71f0d922d6c1" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

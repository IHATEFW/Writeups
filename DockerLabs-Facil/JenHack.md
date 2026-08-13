
# JENHACK

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="858" height="633" alt="jen1" src="https://github.com/user-attachments/assets/9328a0f3-de35-494b-b46f-0612b451c0fc" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen varios puertos abiertos, el 80, 443 y 8080, todos relacionados al servicio HTTP.

<img width="1161" height="452" alt="jen2" src="https://github.com/user-attachments/assets/ae7eb28d-1bfc-410b-bc53-5186df653b4f" />

Una vez tenemos identificados los puertos abiertos, seguiremos enumerando con la herramienta nmap, pero en esta ocasión, le indicaremos a nmap que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos encuentre la versión de dicho servicio HTTP, una vez ejecutado, podemos darnos cuenta que encontramos el titulo de la página web que corre detrás del puerto 80, se llama "Hacker - Nexus" y hace referencia a un dominio llamado jenkhack.hl

<img width="1001" height="628" alt="jen3" src="https://github.com/user-attachments/assets/db5fc304-4a42-49c5-9fb5-d101db367584" />

Tambien podemos encontrar que en el puerto 8080 existe un servicio Jetty, por lo cual, podemos darnos cuenta que corre un Jenkins por detrás.

<img width="870" height="628" alt="jen4" src="https://github.com/user-attachments/assets/ac9e80be-a21f-41a4-816c-ce2b142af0c3" />

Validamos la página web del puerto 80 y no se ve nada interesante.

<img width="1238" height="628" alt="jen5" src="https://github.com/user-attachments/assets/5fa65c39-bec3-4627-a29b-74d48535d0ee" />

<img width="1238" height="628" alt="jen6" src="https://github.com/user-attachments/assets/ddf19f36-becc-4077-83db-b972d67a21ca" />

Procederemos a incluir en nuestro archivo /etc/hosts el dominio que encontramos jenkhack.hl, lo guardamos.

<img width="740" height="380" alt="jen7" src="https://github.com/user-attachments/assets/e56beca4-5d6f-40b4-bcc6-439f5127b844" />

Buscamos en el navegador dicho dominio y nos redirecciona al típico login de autenticación de Jenkins.

<img width="1149" height="603" alt="jen8" src="https://github.com/user-attachments/assets/61f3d4f3-cdd1-48d1-8297-0afb08acf38e" />

Como no tenemos credenciales válidas, nos acordamos que nos faltó revisar el codigo fuente de la página web que corre por detrás el puerto 80, lo revisamos y encontramos una credencial.

<img width="1149" height="603" alt="jen9" src="https://github.com/user-attachments/assets/2b1a7658-d48f-4552-8c34-ce7c5c37a4bf" />

La probamos y logramos acceder al panel de admin al interior del Jenkins.

<img width="1223" height="612" alt="jen10" src="https://github.com/user-attachments/assets/361d3bb4-8217-4a7c-b9bb-ca41657b7868" />

Nos dirigiremos a admin > configuration y vemos que cada vez que se ejecuta un pod se ejecuta con un comando, en este caso "whoami", lo cual pensamos de inmediato en modificarlo para incluir código malicioso

<img width="1008" height="628" alt="jen11" src="https://github.com/user-attachments/assets/c9df020e-1b6f-48d9-be93-740e784ccdf7" />

<img width="436" height="628" alt="jen12" src="https://github.com/user-attachments/assets/455752d5-f21e-4812-a262-3c0789ce9809" />

Nos pondremos en escucha con la herramienta netcat por el puerto 443 (puede ser el de vuestra preferencia).

<img width="616" height="184" alt="jen13" src="https://github.com/user-attachments/assets/8bb4be00-db5e-4130-8673-df075a717b6a" />

Inyectamos el típico oneliner de una reverse shell de bash y lo guardamos.

<img width="943" height="618" alt="jen14" src="https://github.com/user-attachments/assets/1211d226-f7fd-4eea-89dc-4552802460e4" />

Lo ejecutamos en "Construir Ahora". 

<img width="384" height="618" alt="jen15" src="https://github.com/user-attachments/assets/6f3073e3-efce-4321-aac7-5bb944578c50" />

Revisamos nuestra escucha y ¡Ganamos acceso a la máquina víctima!

<img width="708" height="355" alt="jen16" src="https://github.com/user-attachments/assets/e382f481-48c9-4290-b55f-2bdbead120ad" />

<img width="680" height="623" alt="jen17" src="https://github.com/user-attachments/assets/12c7781d-e678-4d4e-82ac-2ee4e74ba2ae" />

<img width="546" height="341" alt="jen18" src="https://github.com/user-attachments/assets/e9073642-a3ac-4abf-8a58-fd31061a4341" />

<img width="999" height="524" alt="jen19" src="https://github.com/user-attachments/assets/28929127-2a9f-4175-8b65-8ee473fe05b1" />

<img width="748" height="454" alt="jen20" src="https://github.com/user-attachments/assets/ffd83ab9-2bac-4e1a-9026-9276d131f8e2" />

<img width="565" height="363" alt="jen21" src="https://github.com/user-attachments/assets/b6203f21-533e-4b51-b36b-4e5dfa3c3c83" />

<img width="513" height="308" alt="jen22" src="https://github.com/user-attachments/assets/7797119f-f99a-47fd-a219-107847bd23fe" />

<img width="771" height="495" alt="jen23" src="https://github.com/user-attachments/assets/44afaa64-8cdb-4ac3-b0d1-e0417f91504a" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

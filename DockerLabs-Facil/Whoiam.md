
# WHOIAM

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1051" height="911" alt="who1" src="https://github.com/user-attachments/assets/7aa3f0f9-7128-47bc-96d7-7e8791d941ec" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que solo existe el puerto 80, relacionado al servicio HTTP.

<img width="1501" height="591" alt="who2" src="https://github.com/user-attachments/assets/ae6de02c-625b-4d9d-8f67-cfa3601ee7ea" />

Una vez ya tenemos el único puerto abierto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio, esto de la siguiente manera, una vez ejecutado, podemos visualizar que al parecer nos enfrentaremos a un CMS (gestor de contenido), llamado Wordpress, además, podemos ver el http-title, llamado Whoiam.

<img width="1196" height="721" alt="who3" src="https://github.com/user-attachments/assets/630691d8-231e-478b-8259-ed874cdfe5f1" />

Lanzaremos el comando Whatweb para ver las tecnologias que se están empleando detrás de la página web, a su vez, lanzaremos un ataque de fuerza bruta con la herramienta Gobuster para poder identificar directorios ocultos, una vez ejecutado, nos llama la atención varios directorios que luego los revisaremos, de momento, nos iremos por el /index.php

<img width="1904" height="896" alt="who4" src="https://github.com/user-attachments/assets/05942a6e-c386-4ce0-a122-edec9126bba1" />

Dentro del /index.php, vemos la página web hecha en Wordpress, nada interesante.

<img width="1613" height="616" alt="who5" src="https://github.com/user-attachments/assets/080656c2-8766-4c54-a54a-566a919a22b8" />

Ahora revisaremos el /wp-admin que nos encontramos realizando el ataque de fuerza bruta, el cual automaticamente nos redigire a /wp-login.php, que básicamente es el típico login de autenticación que tiene Wordpress.

<img width="1574" height="866" alt="who6" src="https://github.com/user-attachments/assets/64b8bc40-bbfb-4156-bccd-660eb54fe5a5" />

Como no tenemos credenciales válidas, ejecutaremos la herramienta Wpscan para enumerar usuarios válidos del sistema, esto con la siguiente combinatoria de comandos.

<img width="1574" height="866" alt="who7" src="https://github.com/user-attachments/assets/ae7c0314-c948-4283-b8f2-729dc9caefac" />

Una vez finalizado el escaneo, nos encuentra 2 usuarios válidos, "erik" y "developer"

<img width="1361" height="907" alt="who8" src="https://github.com/user-attachments/assets/b4c390f1-6c19-4c75-9efd-13fe348b1243" />

En este punto, revisaremos el directorio /backups que tambien anteriormente encontramos, el cual nos expone un archivo .zip que nos descargaremos en nuestra máquina atacante.

<img width="764" height="492" alt="who9" src="https://github.com/user-attachments/assets/3cc6d6b3-e104-4b58-b3d5-bc11b612212b" />

Descomprimiéndolo con el comando unzip, vemos su contenido y tiene las credenciales válidas del usuario "developer" para ingresar al dashboard de Wordpress.

<img width="764" height="492" alt="who10" src="https://github.com/user-attachments/assets/706a317f-9334-4c31-85a9-c21646975e8d" />

Logramos ingresar con éxito al dashboard de Wordpress.

<img width="1906" height="895" alt="who11" src="https://github.com/user-attachments/assets/1d4c13a4-0e96-42c5-bd6e-bbff97b9f9bf" />

En este punto, nos vamos al apartado donde dice "Plugins" para revisar posibles plugins de los cuales podríamos abusar, nos llama la atención un plugin desactualizado, llamado "Modern Events Calendar Elite".

<img width="1625" height="797" alt="who12" src="https://github.com/user-attachments/assets/2ddc2289-50f4-4556-810c-d6caa4837595" />

Volvemos a la terminal y buscamos el la base de datos de exploitdb, con el comando searchsploit y filtrando por "Modern Events Calendar Elite", nos muestra 2 resultados válidos, entre ellos un RCE en un script de python.

<img width="1852" height="400" alt="who13" src="https://github.com/user-attachments/assets/2b95e768-9141-4d3b-8bf6-31dfc2cbdcbc" />

Lo descargamos y lo ejecutamos de la siguiente manera, con el cual conseguimos cargar un archivo llamado shell.php en un directorio del sistema, esto se supone que nos dará acceso al RCE.

<img width="1657" height="902" alt="who14" src="https://github.com/user-attachments/assets/4b9a0f8d-ee05-4675-8a83-6f6919906787" />

Revisamos la ruta donde se subió el archivo y efectivamente logramos levantar una especie de "terminal" interactiva. 

<img width="1657" height="902" alt="who15" src="https://github.com/user-attachments/assets/14ada3ec-4d76-42a8-aacf-a8a24e30f74e" />

Probamos el típico oneliner de reverse shell de bash para poder lanzarnos la reverse shell que nos dará acceso a la máquina víctima.

<img width="1657" height="902" alt="who16" src="https://github.com/user-attachments/assets/79dee909-1535-4c0c-ad99-18f4d6347eb0" />

Sin antes ponernos en escucha con la herramienta netcat por el puerto 443, la lanzamos y ¡ganamos acceso a la máquina víctima!

<img width="981" height="385" alt="who17" src="https://github.com/user-attachments/assets/adf3859e-1a63-4129-ad06-55913742f807" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```

<img width="900" height="757" alt="who18" src="https://github.com/user-attachments/assets/c521c7d5-0351-4310-9260-bc3788aa0ef4" />

<img width="1433" height="338" alt="who19" src="https://github.com/user-attachments/assets/09366746-002a-4606-8507-1431c8e29adc" />

<img width="1615" height="722" alt="who20" src="https://github.com/user-attachments/assets/8d26c2e2-4ca7-4432-93eb-f859c02f323c" />

<img width="1412" height="415" alt="who21" src="https://github.com/user-attachments/assets/6d95dfb9-c979-4c73-884c-446c1fc85db8" />

<img width="1537" height="758" alt="who22" src="https://github.com/user-attachments/assets/50cdd257-0837-49d4-88d0-1a246b925b95" />

<img width="1418" height="593" alt="who23" src="https://github.com/user-attachments/assets/07d73823-312c-4694-8101-d01765b03742" />

<img width="1390" height="893" alt="who24" src="https://github.com/user-attachments/assets/3e77f025-bdd7-4be8-b18d-6bd8dfde5607" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

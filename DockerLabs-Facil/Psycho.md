La máquina Psycho de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como podemos explotar un LFI con un parámetro que encontramos realizando fuzzing, logrando luego leer el id_rsa de una usuario válido del sistema para conectarnos por SSH, luego dentro de la máquina víctima pivotamos entre usuarios con permisos a nivel de sudoers hasta llegar a root . .

# PSYCHO

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1057" height="866" alt="psycho1" src="https://github.com/user-attachments/assets/f2cee9b2-8f6b-4e49-9f4f-717929bde125" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1514" height="612" alt="psycho2" src="https://github.com/user-attachments/assets/bdb0bfd4-f6a9-4267-ab8f-b0018d6e1b20" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "4You".

<img width="1437" height="872" alt="psycho3" src="https://github.com/user-attachments/assets/0477553b-c494-4e04-be89-51b3955670c5" />

En este punto, ejecutaremos una ataque de fuerza bruta con la herramienta Gobuster, esto para encontrar posibles directorios ocultos, una vez ejecutado, podemos visualizar un /index.php y un /assets, pero nada más interesante.

<img width="1891" height="912" alt="psycho4" src="https://github.com/user-attachments/assets/1eed1ec6-0dd2-4245-97ce-f99614ee280b" />

Vamos a revisar la página web, donde se expone un posible usuario válido llamado "Luisillo".

<img width="1891" height="912" alt="psycho5" src="https://github.com/user-attachments/assets/14631750-121a-4a5f-825c-918f8aed4e17" />

## 💣 EXPLOTACIÓN

Ejecutaremos un fuzzing con la herramienta wfuzz para encontrar algún posible parametro válido que podamos concatenarle al /index.php, para ver si podemos efectuar algún LFI, una vez ejecutado, podemos visualizar que encontramos el parametro "secret"

<img width="1891" height="912" alt="psycho6" src="https://github.com/user-attachments/assets/fe83fe6a-b04b-408e-b098-fb9391be4817" />

Se nos ocurre ocuparlo para poder visualizar el archivo /etc/passwd de la máquina víctima a tráves de un LFI, y efectivamente podemos, logrando exponernos 2 usuarios válidos, "luisillo" y "vaxei".

<img width="881" height="922" alt="psycho7" src="https://github.com/user-attachments/assets/01243d66-2a81-43b8-ad27-0198d8ec1ede" />

Como ya podemos efectuar un LFI, vamos a leer el archivo id_rsa del usuario vaxei, ya que intentando con usuario luisillo no funcionó.

<img width="881" height="922" alt="psycho8" src="https://github.com/user-attachments/assets/c046cfc5-4a58-48f6-ae33-54fc24ff64d0" />

Nos copiamos el private key en un archivo que llamaremos id_rsa en nuestra máquina atacante, le daremos permisos 600 y nos conectaremos por SSH, ¡Logrando acceso a la máquina víctima!

<img width="961" height="471" alt="psycho9" src="https://github.com/user-attachments/assets/fdcfe5af-27f9-4e28-b634-5d9b351101ed" />

## 🔑 ESCALADA DE PRIVILEGIOS

Dentro de la máquina víctima, daremos el comando sudo -l para ver si podemos ejecutar algun binario/comando con permisos a nivel de sudoers y podemos ejecutar el lenguaje perl como el usuario luisillo.

<img width="1396" height="330" alt="psycho10" src="https://github.com/user-attachments/assets/60ba9ffe-5528-43b6-97f0-79155c97bbaf" />

Nos dirigiremos a la web gtfobins.org para filtrar por "perl" y copiarnos el primer comando.

<img width="1370" height="627" alt="psycho11" src="https://github.com/user-attachments/assets/a2aa1e42-bf6e-4fee-9065-8bca3c9ad046" />

Lo lanzamos y pivotamos con éxito al usuario luisillo, ahora nuevamente daremos el comando sudo -l y vemos que podemos ejecutar como root un script de python3.

<img width="1405" height="279" alt="psycho12" src="https://github.com/user-attachments/assets/40e196b1-4006-4262-85ff-074fe14f540e" />

Revisaremos dicho script y vemos que se exportan algunas librerías, donde podremos ejecutar un Python Library Hijacking, pero lo haremos más fácil.

<img width="750" height="700" alt="psycho13" src="https://github.com/user-attachments/assets/8b92547b-19de-49a6-aaeb-8be61ec676d2" />

Como tenemos permisos de escritura en el directorio /opt, vamos a mover todo el contenido de paw.py a otro archivo y nos crearemos otro paw.py pero con código malicioso, este código lo que hará es modificar el binario /bin/bash para convertirlo en SUID, ahora lo ejecutamos y nos lanzamos una shell privilegiada y somo root, ¡máquina hackeada! . .

<img width="964" height="402" alt="psycho14" src="https://github.com/user-attachments/assets/167a1aa9-464c-463a-8052-c7734db24daa" />

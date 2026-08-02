La máquina WalkingCMS de la plataforma Dockerlabs.es, es una máquina de dificultad "Fácil", la cual nos enseña como abusar de un gestor de contenido CMS llamado WordPress, utilizando la herramienta Wpscan para encontrar credenciales válidas, una vez dentro del dashboard, nos aprovechamos de un tema para editar un archivo .php de su configuración y lanzarnos una reverse shell, ya dentro de la máquina víctima logramos pivotar a root gracias a un binario expuesto con permisos SUID. .

# WALKINGCMS

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="812" height="620" alt="walkingcms1" src="https://github.com/user-attachments/assets/e8d3daae-8004-42e0-aa61-ba3c4fbf58db" />

## 🔎 ENUMERACIÓN

Cuando ya tengamos la ip de la máquina víctima, se procederá a realizar un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos existentes abiertos/expuestos que tenga la máquina, esto con el siguiente comando, una vez ejecutado, podemos visualizar que solamente tenemos el puerto 80 abierto, relacionado al servicio HTTP.

<img width="1174" height="602" alt="walkingcms2" src="https://github.com/user-attachments/assets/4fd4cccf-9b7d-44db-b68f-3d1c32895da2" />

Ya cuando tengamos los puertos abiertos identificados, seguiremos realizando un escaneo pero más exhaustivo con nmap, esta vez indicandole que nos enumere la versión de dicho servicio HTTP y que nos arroje un conjunto básico de scripts de reconocimiento, de la siguiente manera, una vez ejecutado, vemos que solo encontramos la típica web por defecto de Apache2, no hay nada más interesante.

<img width="911" height="481" alt="walkingcms3" src="https://github.com/user-attachments/assets/91323fef-7b36-407c-8273-5dd14cb6393e" />

En este punto, vamos a ejecutar un ataque de fuerza bruta de directorios con la herramienta Gobuster, esto para lograr identificar directorios escondidos detrás de la /, una vez ejecutado, podemos visualizar con nos encontró el directorio /wordpress, ya sabemos que nos enfrentamos a un gestor de contenido (CMS).

<img width="1344" height="369" alt="walkingcms4" src="https://github.com/user-attachments/assets/67b80e35-c069-422f-95df-023f94348302" />

Vamos a revisar dicho directorio y vemos que una web diseñada por wordpress, que dice "Web Invulnerable".

<img width="1230" height="631" alt="walkingcms5" src="https://github.com/user-attachments/assets/ee85ad40-a548-4bb5-b33b-d1fcaa5311a6" />

Sabemos que en un gestor de contenido como wordpress existen directorios típicos como es el caso de /wp-login.php, por lo tanto, accederemos a él. 

<img width="1230" height="631" alt="walkingcms6" src="https://github.com/user-attachments/assets/401ef7fb-3968-4ca4-9219-6fe13de81136" />

Como no tenemos credenciales válidas, vamos a utilizar la herramienta Wpscan, para enumerar usuarios válidos del sistema, esto de la siguiente manera:

<img width="896" height="631" alt="walkingcms7" src="https://github.com/user-attachments/assets/716da09e-1eaa-41c2-a10a-bc2248f58d0f" />

Una vez finalice el escaneo, podemos ver que nos encontró el usuario "mario".-

<img width="896" height="631" alt="walkingcms8" src="https://github.com/user-attachments/assets/e47933d7-0359-4302-a525-5907d1bcd5bc" />

En esta ocasión, vamos a seguir enumerando con la herramienta Wpscan, pero esta vez utilizando el usuario mario y otorgándole un diccionario de contraseñas como es el rockyou.txt, para que nos encuentre su contraseña válida, esto de la siguiente manera:

<img width="1249" height="631" alt="walkingcms9" src="https://github.com/user-attachments/assets/5f9942c6-f36e-4168-b9fe-e46093edf8ba" />

Una vez finaliza el escaneo, podemos ver que nos encontró la contraseña válida.

<img width="536" height="631" alt="walkingcms10" src="https://github.com/user-attachments/assets/666bf445-e5f8-4cf5-947a-357ca93eb14d" />

## 💣 EXPLOTACIÓN

La probamos en el login de autenticación anteriormente visto y ¡Logramos acceder al dashboard!

<img width="1238" height="631" alt="walkingcms11" src="https://github.com/user-attachments/assets/e4c627bb-3bbb-40d5-ac75-0a4475c3b322" />

Nos dirigiremos al apartado de "Apariencia/Temas", y utilizaremos el tema Twenty-Twenty-Two.

<img width="1196" height="631" alt="walkingcms12" src="https://github.com/user-attachments/assets/007cadaa-879b-4737-98e1-77ec281152d5" />

Lo presionamos y nos vamos a la opción de "Theme Code Editor" para intentar encontrar algun archivo .php que podamos modificar e inyectar código malicioso.

<img width="1196" height="631" alt="walkingcms13" src="https://github.com/user-attachments/assets/149a3ee4-d381-4112-ba4b-198006853dec" />

Vamos a aprovecharnos del index.php que mostraré a continuación, para modificar su contenido e inyectar una reverse shell de php.

<img width="1278" height="631" alt="walkingcms14" src="https://github.com/user-attachments/assets/ca82a578-1b2b-4d30-8a2c-54fd3faf1d6d" />

Nos vamos a la web revshells.com para escoger la reverse shell de php PentestMonkey, la seteamos con nuestra ip de máquina atacante y con algún puerto que queramos ponernos a la escucha, la copiamos.

<img width="1278" height="631" alt="walkingcms15" src="https://github.com/user-attachments/assets/b25fb29c-900a-482b-a8a7-7e34d6094355" />

La pegamos en el index.php que estamos modificando y le damos "Update File".

<img width="1278" height="631" alt="walkingcms16" src="https://github.com/user-attachments/assets/e8c3a95a-6c8f-403a-8239-34ae33201fc6" />

Nos pondremos en una terminal en escucha con la herramienta netcat por el puerto 445.

<img width="639" height="190" alt="walkingcms17" src="https://github.com/user-attachments/assets/f12a7d0a-64d9-4d41-a0aa-c758b21a4a1d" />

Nos vamos al siguiente directorio y vemos que la web queda cargando, esa es una señal.

<img width="900" height="202" alt="walkingcms18" src="https://github.com/user-attachments/assets/0c203493-5195-45ab-8cf2-01b7168715a7" />

Verificamos el listener y vemos que ¡Ganamos acceso a la máquina víctima! 🔥, ya estamos dentro.

<img width="1047" height="375" alt="walkingcms19" src="https://github.com/user-attachments/assets/210d7af7-a927-46b2-b5f4-a3c4fa0a2517" />

Ya en la máquina víctima, procederemos a realizar tratamiento de la TTY, para que tengamos una terminal estable, que podamos ejecutar CTRL + L y se nos limpie la pantalla, que podamos ejecutar CTRL + C y la reverse shell no se caíga, esto lo haremos con los siguientes comandos:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
```
## 🔑 ESCALADA DE PRIVILEGIOS

Ya con la tty estable, procederemos a revisar con el siguiente comando si existen binarios con permisos SUID para ejecutar con privilegios de root y efectivamente encontramos el binario /bin/env

<img width="522" height="356" alt="walkingcms20" src="https://github.com/user-attachments/assets/029b6c31-abf3-418c-8636-81400e50d3b2" />

Nos vamos a nuestra biblia gtfobins.org y filtramos por "env", nos vamos al apartado de SUID y seleccionamos el primer comando.

<img width="1136" height="606" alt="walkingcms21" src="https://github.com/user-attachments/assets/158bcfb5-3818-4e37-af36-a89f350f6906" />

Lo lanzamos y listo, ¡Logramos pivotar al usuario root!, máquina hackeada 🔥

<img width="1136" height="606" alt="walkingcms22" src="https://github.com/user-attachments/assets/3865d36d-9bf9-402c-b51a-5f0a7535ef7f" />

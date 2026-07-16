
# INTERNAL

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="732" height="603" alt="internal1" src="https://github.com/user-attachments/assets/295500d9-fe26-481c-8bb0-e90e66ee9871" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen dos puertos abiertos, el 22 correspondiente al servicio SSH y el puerto 80 correspondiente a HTTP.

<img width="781" height="545" alt="internal2" src="https://github.com/user-attachments/assets/9c4b3827-0f96-44a8-ace9-1fb8476173c7" />

Como ya tenemos en nuestras manos los puertos abiertos, procederemos a seguir enumerando con nmap, pero esta vez, indicandole a nmap que nos encuentre la versión de dichos servicios, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, una vez ejecutado el escaneo, encontramos que nos está redirigiendo al dominio internal.dl

<img width="837" height="636" alt="internal3" src="https://github.com/user-attachments/assets/2e3a83b3-6904-43fc-a7e8-c44dbb089460" />

Al cual si accedemos, nuestra máquina no sabe como resolver, por lo tanto, tendremos que agregar dicho dominio con su ip a nuestro archivo /etc/hosts.

<img width="1086" height="636" alt="internal4" src="https://github.com/user-attachments/assets/dc491c23-1a92-4515-910c-425f249da523" />

Lo agregamos de la siguiente manera:

<img width="740" height="336" alt="internal5" src="https://github.com/user-attachments/assets/02f6201b-96a4-482d-a35e-3cdf3c946803" />

Una vez ya tenemos el dominio en el /etc/hosts, procederemos a recargar la web, y listo, nos aparece la verdadera web del puerto 80, la examinamos, pero no encontramos nada interesante de lo cual podemos aprovecharnos.

<img width="1246" height="635" alt="internal6" src="https://github.com/user-attachments/assets/78aa9e0b-4204-4f12-94be-15f97aad761f" />

En este punto, procederemos a realizar un ataque de fuerza bruta de directorios con la herramienta GoBuster, esto para lograr identificar directorios ocultos luego de la /, una vez ejecutado, podemos ver que no encontramos nada importante.

<img width="1199" height="572" alt="internal7" src="https://github.com/user-attachments/assets/bc624d5b-936e-47f3-a62c-f189eddd5cd5" />

Como tenemos un dominio al que nos estamos enfrentando, seguiremos realizando fuzzing, pero esta vez con la herramienta ffuf, indicandole que nos encuentre algo delante de internal.dl, por ejemplo FUZZ.internal.dl, arrojamos el ataque y nos encuentra la palabra "backup" con código 200 válido. 

<img width="1278" height="633" alt="internal8" src="https://github.com/user-attachments/assets/366a60ac-32a5-4e1e-b178-cc82c3d63581" />

Agregamos el nuevo dominio a nuestro archivo /etc/hosts para que nuestra máquina lo resuelva sin inconvenientes.

<img width="782" height="308" alt="internal9" src="https://github.com/user-attachments/assets/859e71e2-301e-4753-aefb-01cac64eca1f" />

Accedemos a dicho nuevo dominio y vemos que la web es diferente, existe un campo donde podemos inyectar comandos, o más bien, especificar algun directorio dentro de la máquina víctima.

<img width="1324" height="626" alt="internal10" src="https://github.com/user-attachments/assets/afe7699c-e788-4f5a-87cd-d9cdcafd7d30" />

Intentamos leer el /etc/passwd, pero vemos que hay por detrás una blacklist con comandos no permitidos, pero sin embargo, logramos leer dicho archivo igual pero de la siguiente manera:

<img width="1324" height="626" alt="internal11" src="https://github.com/user-attachments/assets/e89f71d7-a0fe-41fd-ad62-c5457fcb8a67" />

Como ya podemos ejecutar comandos a nivel de sistema, podemos crear un archivo llamado revshell y le metemos la típica reverse shell en bash, para lanzarnos una conexión con netcat a nuestra máquina atacante.

<img width="514" height="270" alt="internal12" src="https://github.com/user-attachments/assets/98d13c59-40a7-404d-8bc2-45b845ae5dc1" />

Levantamos un servidor http con python por el puerto 80.

<img width="514" height="270" alt="internal13" src="https://github.com/user-attachments/assets/4de21f82-2989-4ffc-aac8-c6408bf62fdf" />

Y nos mandamos la revshell a la máquina víctima con el comando wget pero de la misma forma que hemos estamos evadiendo dicha blacklist

<img width="1313" height="608" alt="internal14" src="https://github.com/user-attachments/assets/e1cddf40-7106-440c-9fbb-aae60a86f80c" />

Recibimos el 200 OK, significa que la revshell ya está dentro de la máquina víctima.

<img width="569" height="262" alt="internal15" src="https://github.com/user-attachments/assets/bb755383-039c-4748-91dc-fa63df6129f2" />

<img width="1235" height="610" alt="internal16" src="https://github.com/user-attachments/assets/66098dcb-fc0f-47c2-aa12-6472a1a8bb42" />

<img width="657" height="462" alt="internal17" src="https://github.com/user-attachments/assets/7682b91c-b4b8-488b-bd2e-ae5424fd1997" />

<img width="676" height="567" alt="internal18" src="https://github.com/user-attachments/assets/e35131b5-53a4-4662-8602-5ff660c2a1be" />

<img width="508" height="557" alt="internal19" src="https://github.com/user-attachments/assets/41fcb5bc-0d60-489a-aabf-2b579314e693" />

<img width="777" height="634" alt="internal20" src="https://github.com/user-attachments/assets/6186277b-20d4-49c8-ad43-0567b80f2745" />

<img width="777" height="634" alt="internal21" src="https://github.com/user-attachments/assets/ce69f570-9edd-47e5-a458-38d001834ea3" />

<img width="595" height="345" alt="internal22" src="https://github.com/user-attachments/assets/6fe12717-9c84-4f07-b594-a7801ffdf981" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

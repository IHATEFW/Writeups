
# ALLIEN

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="828" height="629" alt="allien1" src="https://github.com/user-attachments/assets/4502f40b-0924-44d1-bce3-a338ddce7b56" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que existen varios puertos abiertos, entre ellos los puertos 22, 80, 139 y 445, correspondientes los servicios SSH, HTTP y Samba.

<img width="1147" height="473" alt="allien2" src="https://github.com/user-attachments/assets/645d415a-2662-4134-a928-3e01ebcaf234" />

Una vez ya tenemos los puertos abiertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere las versiones de los servicios que corren en dichos puertos, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web del puerto 80, que dice "Login", al parecer es un panel de autenticación.

<img width="1027" height="626" alt="allien3" src="https://github.com/user-attachments/assets/c142294f-66c4-42e3-8374-4a9d723d29aa" />

Lo revisamos y efectivamente nos encontramos un panel de login de autenticación, se prueban distintas técnicas de SQli para intentar bypassearlo, pero no se logra éxito.

<img width="1230" height="621" alt="allien4" src="https://github.com/user-attachments/assets/c821a5d4-9d1f-430f-b6dd-f86938c64fba" />

En este punto, nos iremos a la terminal nuevamente, para efectuar un ataque de fuerza bruta de directorios con la herramienta Gobuster, esto para lograr identificar posibles directorio ocultos detrás de la /, una vez ejecutado, encontramos el directorio /productos.php

<img width="1333" height="621" alt="allien5" src="https://github.com/user-attachments/assets/532e0ec3-dc85-44b7-947b-64410057f387" />

Lo revisamos y vemos una especie de página que se ofrecen distintos productos, algo como un shop.

<img width="1230" height="621" alt="allien6" src="https://github.com/user-attachments/assets/db51a15f-d689-4fec-9295-7840e899697c" />

Scrolleando se puede identificar 3 campos para ingresar nombre, correo y algún mensaje, se prueba inyectar cosas pero sin éxito.

<img width="1230" height="621" alt="allien7" src="https://github.com/user-attachments/assets/67eb994b-f23b-4fc9-922b-889f3a31b945" />

En este punto, volvemos a la terminal, y vamos a tirar por los puertos de Samba 139 y 445, por lo tanto, vamos a enumerarlos, esto con la herramienta Enum4linux, tirando el comando enum4linux 172.17.0.2, una vez termina el escaneo, vemos que se exponen unas carpetas compartidas, de las cuales solo tenemos lectura y escritura de /myshare

<img width="1177" height="621" alt="allien8" src="https://github.com/user-attachments/assets/2d421079-e5a4-4ce9-80f2-2c218d2ebbf6" />

Bajando, podemos ver que se exponen varios usuarios válidos del sistema, entre ellos está satriani7 que nos llama la atención.

<img width="1177" height="621" alt="allien9" src="https://github.com/user-attachments/assets/3e97453d-de87-4aa2-bb4d-9d00f7fe487f" />

Ahora ocuparemos la herramienta netexec para adjuntarle dicho usuario y realizar una enumeración de contraseña con el diccionario rockyou.txt

<img width="1332" height="621" alt="allien10" src="https://github.com/user-attachments/assets/88c8d5f7-cc6e-4ae8-a597-61359f842312" />

Y ¡Logramos encontrar la password!

<img width="1021" height="144" alt="allien11" src="https://github.com/user-attachments/assets/1638e79b-5d14-4460-9fe6-fb7a2513e615" />

Nos conectamos vía samba con la herramienta smbclient de la siguiente manera y encontramos un archivo llamado access.txt, lo traemos a nuestra máquina atacante.

<img width="935" height="416" alt="allien12" src="https://github.com/user-attachments/assets/e62e8e61-77d8-419c-bd50-748d70b29152" />

Lo leemos y vemos que se trata de un Json Web Token (JWT).

<img width="1342" height="502" alt="allien13" src="https://github.com/user-attachments/assets/e52b9df2-de6a-47b9-9ef4-ba3e76b46037" />

Buscaremos en google alguna herramienta de JWT decode para decodear dicho token y vemos que nos encontró un correo electronico con un dominio.

<img width="1281" height="630" alt="allien14" src="https://github.com/user-attachments/assets/62edfc5e-a754-4f35-a0c4-72fed465eb0a" />

<img width="1142" height="630" alt="allien15" src="https://github.com/user-attachments/assets/e320be36-bbbc-48f4-bf93-0983cab0f1ef" />

<img width="801" height="626" alt="allien16" src="https://github.com/user-attachments/assets/c1067a25-c27e-4de8-b66f-f5c490622c54" />

<img width="731" height="468" alt="allien17" src="https://github.com/user-attachments/assets/3d174017-c5ff-4cee-998c-5d07f02c7a62" />

<img width="737" height="404" alt="allien18" src="https://github.com/user-attachments/assets/f8b2e116-f574-476f-9ec9-ce3f1ba48937" />

<img width="1282" height="463" alt="allien19" src="https://github.com/user-attachments/assets/946d887a-8788-4b3d-a04d-3f97d1934699" />

<img width="606" height="190" alt="allien20" src="https://github.com/user-attachments/assets/ba87ff8b-bcfd-4771-9ad5-779db2966341" />

<img width="1151" height="206" alt="allien21" src="https://github.com/user-attachments/assets/8c5b55a4-8e8c-4fed-8812-0467992a2a98" />

<img width="1143" height="155" alt="allien22" src="https://github.com/user-attachments/assets/2194e549-ac10-4da9-8a51-a2d946dc4e55" />

<img width="1178" height="613" alt="allien23" src="https://github.com/user-attachments/assets/990a1ac2-6e4c-4a0d-aced-bcce5026864a" />

<img width="537" height="315" alt="allien24" src="https://github.com/user-attachments/assets/58b697f8-6380-4d0a-ac0b-4dac197ac755" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

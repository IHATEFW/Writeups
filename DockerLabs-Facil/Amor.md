
# AMOR

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="579" height="366" alt="amor1" src="https://github.com/user-attachments/assets/215faabd-aedd-4970-a167-a923c0782783" />

## 🔎 ENUMERACIÓN

Una vez ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para conocer los puertos abiertos/expuestos que posee dicha máquina, esto se realizará con el siguiente comando, una vez ejecutado, podemos ver que exísten los puertos abiertos 22 y 80, relacionados a los servicios SSH y HTTP.

<img width="1038" height="635" alt="amor2" src="https://github.com/user-attachments/assets/2f723c44-4b7b-47ea-80bb-629c4b7f9950" />

Seguiremos enumerando con la herramienta nmap, pero esta vez realizaremos un escaneo de manera más exhaustiva, indicandole a nmap que nos enumere la versión de dichos servicios, como tambien, que nos arroje un conjunto básico de scripts de reconocimiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar que existe una web llamada "SecurSEC S.L" detrás del puerto 80, vamos a revisarla.

<img width="1038" height="635" alt="amor3" src="https://github.com/user-attachments/assets/8b210844-aa7e-4582-b389-b1d91777afc0" />

Una vez dentro de la web, podemos visualizar que existen distintos tipos de "recomendaciones" que se les da a los usuarios de alguna empresa, como actualizar contraseñas, estar atentos a phishing, etc.

<img width="1181" height="628" alt="amor4" src="https://github.com/user-attachments/assets/3064c7ad-1cf5-4acc-9669-76911b96007b" />

Si bajamos un poco, podemos ver un comentario curioso, en el cual se expone el usuario "Juan" y el usuario "Carlota".

<img width="710" height="396" alt="amor5" src="https://github.com/user-attachments/assets/bd40804c-dede-4b57-a732-5c81b0ac58f7" />

## 💣 EXPLOTACIÓN

Una vez ya tenemos posibles usuarios válidos, procederemos a realizar un ataque de fuerza bruta por SSH con la herramienta hydra, logrando identificar la password del usuario "Carlota".

<img width="944" height="510" alt="amor6" src="https://github.com/user-attachments/assets/763f9b0f-26ae-41c3-a768-26e91b5069fc" />

Nos logueamos por SSH y ¡Ganamos acceso a la máquina víctima".

<img width="696" height="451" alt="amor7" src="https://github.com/user-attachments/assets/a92533eb-826f-47da-8922-79e5162f563c" />

## 🔑 ESCALADA DE PRIVILEGIOS

Procederemos a leer el archivo /etc/passwd para ver si existen más usuarios válidos para así pivotar y vemos que existe el usuario "oscar".

<img width="633" height="560" alt="amor8" src="https://github.com/user-attachments/assets/736ead99-4428-4fd4-a6dd-8e0dd64fc172" />

En este punto, nos dirigiremos al directorio /home/carlota, donde leeremos el archivo .bashrc, donde se exponen las configuraciones de la bash de dicho usuario, pero alfinal, vemos un mensaje interesante, indicando que dentro del directorio /vacaciones puede haber una pista.

<img width="1111" height="635" alt="amor9" src="https://github.com/user-attachments/assets/ad611fdb-8897-4a2a-879d-22ae5805515c" />

Nos dirigiremos al directorio /home/carlota/Desktop/fotos/vacaciones y dentro existe una imagen.jpg.

<img width="520" height="561" alt="amor10" src="https://github.com/user-attachments/assets/d0191183-ca04-4950-93ec-9e77c8154634" />

La vamos a revisar con la herramienta steghide para ver si se está aplicando Esteganografía, pero nos da error de permisos al intentar guardar la información que encontró

<img width="684" height="224" alt="amor11" src="https://github.com/user-attachments/assets/ec6903d3-4847-4898-8e5e-07790440c90c" />

Por lo tanto, nos iremos al directorio /tmp y haremos lo mismo, logrando guardar con éxito el archivo secret.txt, lo leemos y es una cadena en base64.

<img width="822" height="377" alt="amor12" src="https://github.com/user-attachments/assets/5dfaa33f-e1b8-4027-8fc9-bb495e3029bf" />

La decodearemos de la siguiente manera y nos muestra una clave en texto claro.

<img width="630" height="202" alt="amor13" src="https://github.com/user-attachments/assets/f20d2884-3ae9-468e-83be-c9385da7180a" />

Intentaremos pivotar al usuario "oscar" con dicha password y ¡Logramos acceder como dicho usuario!

<img width="624" height="271" alt="amor14" src="https://github.com/user-attachments/assets/b4d03e53-3aee-4475-a784-6761b239fe23" />

Ya como el usuario oscar, daremos el comando sudo -l para ver si tenemos privilegios a nivel de sudoers para intentar ejecutar algun binario y vemos que podemos ejecutar comandos en ruby.

<img width="1018" height="265" alt="amor15" src="https://github.com/user-attachments/assets/d402eb5e-068a-444b-947a-7caee5498f4a" />

Nos dirigiremos a la web gtfobins.org y filtraremos por la palabra "ruby", copiaremos el siguiente comando.

<img width="1015" height="599" alt="amor16" src="https://github.com/user-attachments/assets/5c1103e3-560a-4862-9ec4-f4289cf04ed8" />

Lo ejecutaremos como el usuario root y ¡Listo!, ganamos acceso como el usuario de máximos privilegios, máquina hackeada.

<img width="577" height="216" alt="amor17" src="https://github.com/user-attachments/assets/3e42ad2a-0aa6-4d4b-b07d-e8febbe4a801" />

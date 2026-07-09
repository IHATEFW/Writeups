
# LIBRARY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="723" height="371" alt="library1" src="https://github.com/user-attachments/assets/f9940881-695b-4820-ab98-e095b4c23dec" />

## 🔎 ENUMERACIÓN

Una vez ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para conocer los puertos abiertos/expuestos que posee dicha máquina, esto se realizará con el siguiente comando, una vez ejecutado, podemos ver que exísten los puertos abiertos 22 y 80, relacionados a los servicios SSH y HTTP.

<img width="791" height="552" alt="library2" src="https://github.com/user-attachments/assets/76f7f334-6906-4d69-a417-bca2441f1532" />

Una vez que ya tenemos el puerto abierto, procederemos a realizar otro escaneo con nmap, pero esta vez un poco más exhaustivo, indicandole a nmap que nos encuentre la versión de dichos serviciosy que nos arroje un conjunto básico de scripts de reconomiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar que no existe nada interasante, solo la tipica web por defecto de Apache2.

<img width="891" height="573" alt="library3" src="https://github.com/user-attachments/assets/b2a7b1a6-ef0e-41c9-8c1d-ae26f20c9620" />

Por lo tanto, seguiremos enumerando pero esta vez, realizaremos un ataque de fuerza bruta a directorios con la herramienta GoBuster, esto para poder identificar posibles directorios ocultos y ver si existe más información, esto de la siguiente manera, una vez ejecutado, podemos visualizar que nos encontró un index.php, vamos a revisarlo.

<img width="1194" height="600" alt="library4" src="https://github.com/user-attachments/assets/6e05a84b-a694-47ce-895a-17f4203a69d4" />

Ya dentro de index.php, podemos visualizar una cadena de texto, parecida a una posible contraseña, vamos a guardarla.

<img width="949" height="353" alt="library5" src="https://github.com/user-attachments/assets/cfc4f225-19fc-4cb5-97c8-66b883268779" />

En este punto, realizaremos un ataque de fuerza bruta de SSH con la herramienta hydra, esto para poder identificar algun usuario válido que le pertenezca dicha contraseña, una vez ejecutado, podemos visualizar que nos encontró al usuario "carlos".

<img width="1025" height="353" alt="library6" src="https://github.com/user-attachments/assets/9086c565-5972-41e9-87b1-ca2b2acbe9d5" />

Accedemos vía SSH como el usuario carlos, ¡finalmente logramos acceso a la máquina víctima!

<img width="1025" height="267" alt="library7" src="https://github.com/user-attachments/assets/a59c3c49-0162-4af8-891b-fce38d6e5606" />

<img width="546" height="393" alt="library8" src="https://github.com/user-attachments/assets/0cfed223-b2b2-45df-9b31-edfdd114c8e0" />

<img width="512" height="218" alt="library9" src="https://github.com/user-attachments/assets/a3d144f3-9745-4212-a867-117ea2f10b7f" />

<img width="364" height="237" alt="library10" src="https://github.com/user-attachments/assets/86209c20-0c5d-4b4e-b741-54900864d411" />

<img width="586" height="325" alt="library11" src="https://github.com/user-attachments/assets/356c2865-f5f9-4897-8ad0-4afe097400d0" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

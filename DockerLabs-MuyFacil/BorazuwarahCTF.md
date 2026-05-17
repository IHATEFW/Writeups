Tercera máquina de la plataforma dockerlabs.es de dificultad "Muy Fácil", aquí el escenario es distinto, ya que con comandos básicos lograremos tocar Esteganografía, el arte de ocultar información en una imágen 🎭. .

## BORAZUWARAHCTF

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1521" height="568" alt="borazu1" src="https://github.com/user-attachments/assets/4dace356-9bf4-4abc-bf87-e4a24be763e1" />

## 🔎 ENUMERACIÓN

Como ya nos entregaron la ip de la máquina víctima, ahora procederemos con la fase de enumeración, utilizando la herramienta nmap para enumerar los puertos que se encuentren abiertos, con el siguiente comando:

<img width="1903" height="981" alt="borazu2" src="https://github.com/user-attachments/assets/14bf6610-c119-4d3d-8bb9-856a856e887e" />

Vemos que están expuestos los puertos 22 y 80, de ssh y http, seguiremos enumerando un poco más de información con la herramienta nmap, esta vez indicandole que nos enumere la versión y que nos arroje un conjunto básico de script de reconocimiento:

<img width="1526" height="784" alt="borazu3" src="https://github.com/user-attachments/assets/ea7ad509-57f7-49a0-af1d-c3d3ea810d98" />

Ya tenemos las versiones de los servicios, y además, nos muestra en titulo de la página web (del puerto 80), seguiremos enumerando, realizando fuerza bruta de directorios con la herramienta gobuster, esto para descubrir directorios que se encuentran despues del /, con el siguiente comando:

<img width="1526" height="784" alt="borazu4" src="https://github.com/user-attachments/assets/a50ef6f8-5fc8-404f-96e1-d75a79afafeb" />

Pero vemos que no existe nada importante, solo el index.html que es la misma página web que ingresaremos a continuación:

<img width="1337" height="549" alt="borazu5" src="https://github.com/user-attachments/assets/755093e8-c2e3-4d3e-aa60-443b47e39100" />






La máquina Corridor de la plataforma TryHackMe.com es una máquina de dificultad "Fácil", que nos enseña a identificar y explotar una vulnerabilidad de tipo IDOR (Insecure Direct Object Reference), donde cada una de las puertas del pasillo nos redirige a una habitación cuyo endpoint es un hash MD5, al crackear dichos hashes descubrimos que corresponden a los números del 1 al 13, por lo tanto, calculando el hash MD5 del número 0 logramos acceder a una habitación oculta que contiene la flag . .

# CORRIDOR

## 🚀 DESPLIEGUE DE MÁQUINA

Para desplegar una máquina en TryHackMe, tendrás que descargar el archivo de la vpn, que tendrá como extensión .ovpn y te conectas desde tu terminal ejecutando sudo openvpn archivo.ovpn. Una vez establecida la conexión (mensaje Initialization Sequence Completed), ingresas a la habitación que deseas resolver y presionas Start Machine. TryHackMe desplegará una máquina virtual víctima y, cuando termine de iniciarse, te entregará la dirección ip de la máquina víctima.

<img width="1906" height="902" alt="co1" src="https://github.com/user-attachments/assets/0655f772-7190-4cca-9182-0d86902a188d" />

<img width="1906" height="902" alt="co2" src="https://github.com/user-attachments/assets/b0e350ce-44cd-4778-aee4-b00fc08f8b62" />

## 🔎 ENUMERACIÓN

Comenzaremos con la fase de enumeración/reconocimiento, en primera instancia, lanzaremos una traza ICMP con el comando ping a la dirección ip que nos entregaron, esto para ver si la máquina víctima está encendida, vemos que efectivamente está encendida, a su vez le lanzamos un ping -R (Record Route) que nos muestra la ruta que siguen los paquetes, indicándonos que estamos pasando por nodos intermediarios tanto en la ida como en la vuelta, por eso el TTL es 62 y no 64 que indica que nos enfrentamos a una máquina linux.

<img width="1906" height="902" alt="co3" src="https://github.com/user-attachments/assets/23997ad6-2c4a-45e5-94b0-5d84eada8025" />

Realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existe un único puerto abierto, el 80, correspondiente al servicio HTTP.

```bash
nmap -p- --open --min-rate 5000 -sS -vvv -n -Pn -oN puertos 10.65.166.186
```

<img width="1876" height="581" alt="co4" src="https://github.com/user-attachments/assets/75614688-b1a0-4a17-9fe4-b3238975f194" />

Una vez ya tenemos el puerto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dicho servicio, esto de la siguiente manera, una vez ejecutado, podemos visualizar que la web corre sobre Werkzeug httpd 2.0.3 (Python 3.10.2), es decir, se trata de una aplicación web en Python (probablemente Flask), y el http-title nos indica "Corridor".

```bash
nmap -p80 -sCV -vvv -oN recon 10.65.166.186
```

<img width="1910" height="705" alt="co5" src="https://github.com/user-attachments/assets/777b6b1c-3f52-4389-be4a-08abb232dadf" />

Ahora revisaremos la web del puerto 80, en la cual podemos visualizar la imagen de un pasillo con varias puertas a ambos lados y una al fondo, nada más a simple vista.

<img width="1908" height="892" alt="co6" src="https://github.com/user-attachments/assets/4945b582-ece8-45db-a95e-5e1c977d55eb" />

Revisaremos el código fuente de la página con CTRL + U, y vemos algo interesante, la imagen utiliza un mapa de imagen (`<map>`) con 13 áreas, es decir, cada puerta es clickeable, y cada una redirige a un endpoint cuyo nombre es un hash de 32 caracteres hexadecimales, lo cual nos hace sospechar que se trata de hashes MD5.

<img width="1908" height="892" alt="co7" src="https://github.com/user-attachments/assets/f22a3c6c-8255-473a-83b4-c984ca941fac" />

## 💣 EXPLOTACIÓN

Si hacemos click en la primera puerta, nos lleva a la ruta /c4ca4238a0b923820dcc509a6f75849b, que es simplemente una habitación vacía. Si pasamos dicho hash por una herramienta como CrackStation, descubrimos que es el hash MD5 del número 1, y si repetimos lo mismo con el resto de los hashes, vemos que corresponden a los números del 1 al 13, uno por cada puerta. Aquí es donde aparece la vulnerabilidad IDOR (Insecure Direct Object Reference): la aplicación nos da acceso directo a cada habitación mediante un identificador predecible, sin validar si realmente deberíamos poder acceder a ella, por lo tanto, si sabemos cómo se generan los identificadores, podemos intentar acceder a habitaciones que no aparecen en el pasillo.

<img width="1908" height="892" alt="co8" src="https://github.com/user-attachments/assets/6ba1983b-4a00-4075-b1cd-fe260e4e57b7" />

Como las habitaciones van del 1 al 13, probaremos con el número que falta al inicio, el 0, para ello generaremos su hash MD5 desde nuestra terminal con el siguiente comando (el -n es importante para que echo no agregue un salto de línea, ya que eso cambiaría el hash), obteniendo cfcd208495d565ef66e7dff9f98764da.

```bash
echo -n "0" | md5sum
```

<img width="1365" height="312" alt="co9" src="https://github.com/user-attachments/assets/36630892-2768-4632-8f6e-ce0962b75554" />

Ingresamos a la ruta /cfcd208495d565ef66e7dff9f98764da en el navegador y efectivamente, ¡hemos encontrado la habitación oculta!, dentro de ella está la flag, la copiamos y la pegamos en la web de TryHackMe.

<img width="1869" height="905" alt="co10" src="https://github.com/user-attachments/assets/a13b879e-bb02-40db-b477-7f0a71449488" />

La pegamos y listo, máquina hackeada . .

<img width="1869" height="905" alt="co11" src="https://github.com/user-attachments/assets/8604f78d-c79c-4f95-b440-80059100b647" />

## 🛡️ MITIGACIÓN

Hashear un identificador no lo vuelve secreto: un MD5 de un número secuencial se puede calcular o crackear en segundos. Para evitar este tipo de IDOR, la aplicación debe validar en el servidor si el usuario tiene autorización para acceder a cada recurso, y en caso de necesitar identificadores no adivinables, utilizar valores aleatorios (por ejemplo UUIDv4) en lugar de hashes de valores predecibles.

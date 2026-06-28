
# STELLARJWT

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="640" height="575" alt="stellar1" src="https://github.com/user-attachments/assets/5cf3d78d-460a-4f0b-a6ff-7e06f761dc8c" />

## 🔎 ENUMERACIÓN

Lo primero que realizaremos para comprometer esta máquina, es lograr identificar los puertos abiertos/expuestos con la herramienta nmap, esto lo realizaremos con el siguiente comando, una vez ejecutado, podemos visualizar que existen dos puertos abiertos, el 22 y el 80, relacionados a los servicios SSH y HTTP.

<img width="777" height="547" alt="stellar2" src="https://github.com/user-attachments/assets/e5ebc204-313d-4bd9-a09f-e450a455a6b3" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, le indicaremos que nos encuentre la versión de dichos servicios, como tambien, que nos arroje un conjunto básico de scripts de reconomiento, esto de la siguiente manera, una vez ejecutado, podemos visualizar que existe una web detrás del puerto 80, con el titulo "NASA Hackeada", vamos a revisarla.

<img width="867" height="547" alt="stellar3" src="https://github.com/user-attachments/assets/c43e444c-cc6e-4cc7-89fa-92f4d7a9c6a4" />

Antes de revisarla, en paralelo, lanzaremos un escaneo de fuerza bruta de directorios con la herramienta gobuster, esto con el fin de identificar posibles directorios luego de la /, así es como logramos encontrar el directorio /universe.

<img width="969" height="572" alt="stellar4" src="https://github.com/user-attachments/assets/fe1f1f2e-602a-49e4-b655-b3d28360ad04" />

Ya dentro de la web, vemos que está ambientada en la NASA, haciendonos una pregunta de cual es el astronomo alemán que descubrió Neptuno.

<img width="1219" height="639" alt="stellar5" src="https://github.com/user-attachments/assets/0f547197-7b4c-4e5a-a9e2-947a1396419c" />

Revisaremos el directorio /universe, logrando visualizar la imagen del universo.

<img width="1219" height="639" alt="stellar6" src="https://github.com/user-attachments/assets/4e8312fc-a692-4fd4-aa01-96cb6932cd6e" />

Revisaremos el código fuente con CTRL + U, y vemos que encontramos información sobre la historia de la NASA, al final, vemos un Json Web Token escondido.

<img width="1219" height="639" alt="stellar7" src="https://github.com/user-attachments/assets/253d1200-8dcc-4669-a726-087c54e547f2" />

Lo copiamos y lo debugueamos en alguna web, esto buscando en google JWT decoded, lo ingresamos y vemos que encontramos un posible usuario "neptuno".

<img width="1219" height="639" alt="stellar8" src="https://github.com/user-attachments/assets/8538d3ad-d083-4ca8-850c-04d2cf3c0b27" />

<img width="1219" height="639" alt="stellar9" src="https://github.com/user-attachments/assets/3e6bb9c5-e9d5-4d4c-9145-fa6005115d6a" />

<img width="1219" height="639" alt="stellar10" src="https://github.com/user-attachments/assets/06deac1a-d70b-4545-8c47-dbbdb7db65b2" />

<img width="719" height="433" alt="stellar11" src="https://github.com/user-attachments/assets/d9ad1754-5809-4960-bb84-ad85c67e631a" />

<img width="729" height="567" alt="stellar12" src="https://github.com/user-attachments/assets/beeac135-bd23-4dd4-a9f7-0b0971cbc83b" />

<img width="965" height="615" alt="stellar13" src="https://github.com/user-attachments/assets/9f94ab0b-87c3-4ed2-be54-6ca6a1338056" />

<img width="1171" height="615" alt="stellar14" src="https://github.com/user-attachments/assets/8faa6f04-eb0c-40d7-9cb8-c76cac3225fa" />

<img width="1052" height="422" alt="stellar15" src="https://github.com/user-attachments/assets/df0ade67-49a3-4a38-ae5a-49fc9ba69fa7" />

<img width="803" height="179" alt="stellar16" src="https://github.com/user-attachments/assets/2bf9dedd-e3bb-45ac-80f0-2a74f8d6dc6d" />

<img width="1174" height="593" alt="stellar17" src="https://github.com/user-attachments/assets/565a8c0c-772b-4f63-9dc8-43771f95fc17" />

<img width="718" height="519" alt="stellar18" src="https://github.com/user-attachments/assets/570db3ed-3f5f-420f-9baf-1993e9d9499d" />

<img width="452" height="99" alt="stellar19" src="https://github.com/user-attachments/assets/e8e142e9-1767-49da-a2db-d187345ccb33" />

<img width="606" height="412" alt="stellar20" src="https://github.com/user-attachments/assets/9f126abe-df39-4f90-9756-d1db32d1a7ca" />

<img width="614" height="573" alt="stellar21" src="https://github.com/user-attachments/assets/23b6c7a5-545f-4fd5-8687-d7f4a0e101b8" />

<img width="556" height="184" alt="stellar22" src="https://github.com/user-attachments/assets/6bb1b913-9fc1-492e-91e8-8292ee789708" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

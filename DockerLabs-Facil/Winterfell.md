
# WINTERFELL

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="700" height="595" alt="winter1" src="https://github.com/user-attachments/assets/45e136a6-9465-4556-b169-4610c14da8ea" />

## 🔎 ENUMERACIÓN

Para comprometer esta máquina, procederemos a realizar un escaneo de puertos con la herramienta nmap, esto para visualizar los puertos abiertos existentes, lo realizaremos con el siguiente comando, una vez ejecutado, podemos ver que existen varios puertos abiertos, 22, 80, 139 y 445, correspondientes a los servicios SSH, HTTP y Samba (SMB).

<img width="809" height="608" alt="winter2" src="https://github.com/user-attachments/assets/eb7b039f-0c22-4345-816e-ca95319ca676" />

Una vez que ya tenemos los puertos abiertos, seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos enumere la versión de dichos servicios, a su vez, que nos arroje un conjunto básico de scripts de reconocimiento, una vez ejecutado, podemos ver el titulo de la web que está detrás del puerto 80, se llama "Juego de Tronos", como tambien corroboramos que está el servicio Samba corriendo.

<img width="946" height="631" alt="winter3" src="https://github.com/user-attachments/assets/69786cf8-a558-4368-bf0e-ee116190f8ca" />

Ahora procederemos a revisar la web, y efectivamente está ambientada en la serie "Juego de Tronos" de HBO, con la típica musica de fondo; Vemos que nos está mostrando 3 posibles usuarios, jon, arya y daenerys. 

<img width="1192" height="638" alt="winter4" src="https://github.com/user-attachments/assets/5f540fb1-39d7-4716-86a3-b3ff6974906b" />

En este punto realizaremos fuerza bruta de directorios con la herramienta gobuster, esto para encontrar posibles directorios luego de la /, encontramos un directorio llamado /dragon.

<img width="1342" height="564" alt="winter5" src="https://github.com/user-attachments/assets/0b2235b9-83f1-4d54-acc5-749f25c50d9f" />

Lo revisaremos y existe un directorio más llamado "EpisodiosT1"

<img width="606" height="404" alt="winter6" src="https://github.com/user-attachments/assets/d73fa55e-92e8-4e5f-9465-f6daee69c3c3" />

<img width="910" height="450" alt="winter7" src="https://github.com/user-attachments/assets/e2409b28-5720-47af-8ca6-efaaa5acf12a" />

<img width="541" height="432" alt="winter8" src="https://github.com/user-attachments/assets/4d133e7c-1ff3-4d2a-99ed-dbbb684b8b2c" />

<img width="1261" height="585" alt="winter9" src="https://github.com/user-attachments/assets/7e70a8a7-cd2f-44b6-a8d4-9214e234e54b" />

<img width="984" height="329" alt="winter10" src="https://github.com/user-attachments/assets/fe5fa91b-fd04-4e24-a79b-62feebf7a25b" />

<img width="1181" height="162" alt="winter11" src="https://github.com/user-attachments/assets/25fba1c8-c719-474c-9ff4-71eaaa59c12d" />

<img width="926" height="419" alt="winter12" src="https://github.com/user-attachments/assets/06489144-2ea2-49c1-b587-b25ef354712c" />

<img width="297" height="212" alt="winter13" src="https://github.com/user-attachments/assets/245b451e-e1f4-4398-8ea4-1287be7d005c" />

<img width="928" height="353" alt="winter14" src="https://github.com/user-attachments/assets/28a3bda5-7a93-499f-8e6f-695c953b958c" />

<img width="1155" height="413" alt="winter15" src="https://github.com/user-attachments/assets/e57c88ae-3131-49ce-9bc1-37230a6d0d21" />

<img width="933" height="472" alt="winter16" src="https://github.com/user-attachments/assets/6f34297b-06be-418f-8d90-e954960792fe" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

La máquina Trust de la plataforma dockerlabs.es, es de dificultad "Muy Fácil" y es una de las primeras máquinas que recomiendo realizar cuando estas iniciando en el mundo del hacking/pentesting

# TRUST

🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="726" height="544" alt="maquinatrust1" src="https://github.com/user-attachments/assets/f7c9decb-5d8f-4668-adda-d8f0a63f5bb0" />

🔎 ENUMERACIÓN

Tendremos en nuestras manos la ip de la máquina víctima, en este caso la 171.21.0.2, por lo tanto, procedemos a enumerar los puertos abiertos/expuestos con nmap:

<img width="1140" height="789" alt="maquinatrust2" src="https://github.com/user-attachments/assets/c5d5b952-8707-4789-83dd-b844b7ad9f56" />

Vemos que está expuesto el puerto 22 relacionado al servicio ssh y el puerto 80 relacionado al servicio http, seguiremos enumerando un poco más exhaustivo, solicitando a nmap que enumere las versiones de dichos servicios y que arroje un conjunto básico de script de reconocimiento:

<img width="1533" height="788" alt="maquinatrust3" src="https://github.com/user-attachments/assets/653129a0-fa0e-4178-be74-1c51411a966c" />

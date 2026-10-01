
# PKGPOISON

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1237" height="825" alt="pkg1" src="https://github.com/user-attachments/assets/07a45f96-feee-4405-a9e4-3c9e916dc7d2" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen los puertos abiertos 22 y 80, correspondientes a los servicios SSH y HTTP.

<img width="1512" height="601" alt="pkg2" src="https://github.com/user-attachments/assets/31f45800-5129-4350-9556-0fcd9228dbdd" />

Una vez ya tenemos los puertos identificados, seguiremos enumerando con la herramienta nmap, pero esta vez, indicándole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, esto de la siguiente manera, una vez ejecutado, podemos visualizar el titulo de la página web, indicando "404 Not Found".

<img width="1433" height="914" alt="pkg3" src="https://github.com/user-attachments/assets/2c68eda5-6c7c-4972-872e-adc4807cbddb" />

<img width="1788" height="914" alt="pkg4" src="https://github.com/user-attachments/assets/7647a953-25f2-45a3-aac5-ae75f7a5f52b" />

<img width="1788" height="914" alt="pkg5" src="https://github.com/user-attachments/assets/6edfebbf-10c4-4d43-81ce-ade775ccc5ed" />

<img width="953" height="344" alt="pkg6" src="https://github.com/user-attachments/assets/c5f18bc1-0cdb-429a-a0be-24994207e57f" />

<img width="959" height="397" alt="pkg7" src="https://github.com/user-attachments/assets/7e0a721b-604e-415e-b856-f55231dd8f09" />

<img width="1557" height="342" alt="pkg8" src="https://github.com/user-attachments/assets/4ea16edc-ba48-418f-9f7b-76e936c8dac0" />

<img width="869" height="510" alt="pkg9" src="https://github.com/user-attachments/assets/1dc028b7-28cb-48c7-9778-bb9cb3a4a151" />

<img width="994" height="773" alt="pkg10" src="https://github.com/user-attachments/assets/17413a39-9ed1-4dcb-86fd-25ef03ccdac9" />

<img width="1892" height="502" alt="pkg11" src="https://github.com/user-attachments/assets/935b730a-18a0-483c-abfd-fdc769afaed5" />

<img width="1298" height="153" alt="pkg12" src="https://github.com/user-attachments/assets/474f41d5-d474-4850-96ef-d6ba32f3bea2" />

<img width="1333" height="444" alt="pkg13" src="https://github.com/user-attachments/assets/c1e6ddce-57bf-498b-8054-93dcd49e9279" />

<img width="1341" height="172" alt="pkg14" src="https://github.com/user-attachments/assets/5dfc15c6-79dd-431c-928d-357e8b4f566c" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

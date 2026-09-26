
# REDIRECTION

## 🚀 DESPLIEGUE DE MÁQUINA

Una vez descargado el archivo .zip de la plataforma dockerlabs.es, se descomprime con el comando unzip y se despliega de la siguiente manera:

<img width="1173" height="835" alt="redi1" src="https://github.com/user-attachments/assets/e7a4bf6f-65b7-49a4-a518-eaa498764357" />

## 🔎 ENUMERACIÓN

En primera instancia, realizaremos un escaneo de puertos con la herramienta nmap, esto para poder identificar los puertos abiertos/expuestos que tenga la máquina víctima, con el siguiente comando, una vez ejecutado, podemos darnos cuenta que existen 2 puertos abiertos, el 22 correspondiente al servicio SSH y el 80 correspondiente a HTTP.

<img width="1512" height="611" alt="redi2" src="https://github.com/user-attachments/assets/d66bbd69-b426-49e8-a2bf-b948b0316b52" />

Una vez ya tenemos el puerto abierto identificado, seguiremos enumerando con la herramienta nmap, pero esta vez, indicandole que nos arroje un conjunto básico de scripts de reconocimiento, a su vez, que nos enumere la versión de dichos servicios, una vez ejecutado, podemos visualizar en el http-title el titulo de la web, llamado "Laboratorio de Open Redirect".

<img width="1514" height="871" alt="redi3" src="https://github.com/user-attachments/assets/13f3b758-8658-462b-9a86-d32fe40164a4" />

Lo revisamos y podemos ver 3 laboratorios donde podemos practicar dicha vulnerabilidad relacionada al OWASP top 10.

<img width="1514" height="871" alt="redi4" src="https://github.com/user-attachments/assets/9b2f5a6c-236f-4583-8419-ed8f6cf6c55a" />

Probaremos el laboratorio 1, donde dice que si presionamos el enlace seremos redirigido un sitio web.

<img width="1206" height="718" alt="redi5" src="https://github.com/user-attachments/assets/1b8f8de3-f5d0-4d6a-b796-2f22777e291a" />

Nos redirige a google.com

<img width="1206" height="718" alt="redi6" src="https://github.com/user-attachments/assets/04f4478a-9b9a-4581-9949-e10c99db2300" />

Volvemos atrás y revisaremos el código fuente con CTRL + U para ver como se está tramitando esto por detrás, bajamos e identificamos el archivo redirect.php que hace efectivo dicho redirect, lo copiamos.

<img width="800" height="906" alt="redi7" src="https://github.com/user-attachments/assets/6e3128c9-17a8-4651-a942-14a3cd936d86" />

Lo pegamos en la url y cambiamos la web de google por dockerlabs.es para ver si nos deja.

<img width="1029" height="610" alt="redi8" src="https://github.com/user-attachments/assets/517149ea-4b28-41bb-a8d5-9e0270988e84" />

Efectivamente nos redirige a dockerlabs.es, el laboratorio 1 se encuentra completado con este ejercicio.

<img width="1628" height="704" alt="redi9" src="https://github.com/user-attachments/assets/3e7f804e-b421-4253-9957-946e117b5b4d" />

Pasamos al laboratorio 2

<img width="1628" height="704" alt="redi10" src="https://github.com/user-attachments/assets/319c8c94-f7bf-492f-9f77-198c14b4f92e" />

<img width="829" height="319" alt="redi11" src="https://github.com/user-attachments/assets/cd986222-2c81-40be-80e8-721d50662b57" />

<img width="878" height="295" alt="redi12" src="https://github.com/user-attachments/assets/b29136e1-aaa2-4e29-858c-4a382f5ce6b0" />

<img width="1706" height="656" alt="redi13" src="https://github.com/user-attachments/assets/70966889-b71d-4641-a204-6bc43dd305ae" />

<img width="1527" height="759" alt="redi14" src="https://github.com/user-attachments/assets/26b8ad59-173e-4726-a74d-afac488348cd" />

<img width="1089" height="341" alt="redi15" src="https://github.com/user-attachments/assets/452eabb3-007a-4d19-ae79-23083395dec7" />

<img width="874" height="291" alt="redi16" src="https://github.com/user-attachments/assets/ca84e80c-603d-43e0-b295-888c21926593" />

<img width="1383" height="744" alt="redi17" src="https://github.com/user-attachments/assets/46bdf1fc-d885-4745-9036-c6db5c72fe39" />

<img width="1015" height="607" alt="redi18" src="https://github.com/user-attachments/assets/327aebed-8c69-48a2-9c0f-a7396ecb0b5f" />

<img width="1165" height="581" alt="redi19" src="https://github.com/user-attachments/assets/7d1a76f7-5732-4ca0-ac54-40a7d735aba0" />

<img width="845" height="770" alt="redi20" src="https://github.com/user-attachments/assets/f5dfeb89-c219-473d-b133-e627d312db00" />

<img width="681" height="844" alt="redi21" src="https://github.com/user-attachments/assets/5885a251-e33e-4cae-b937-6fffe9bc1341" />

<img width="1297" height="313" alt="redi22" src="https://github.com/user-attachments/assets/07bf7cc4-8ad7-4dfa-ad59-d19f2f11c58a" />

<img width="856" height="871" alt="redi23" src="https://github.com/user-attachments/assets/8dd7db0f-03b8-45fa-84bd-70240ee45118" />

<img width="741" height="485" alt="redi24" src="https://github.com/user-attachments/assets/5267c7ca-c7ed-4c99-b61a-0d0d56c2c6c2" />

## 💣 EXPLOTACIÓN

## 🔑 ESCALADA DE PRIVILEGIOS

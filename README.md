# WriteUp Maquina Trust

Primero haremos un Ping a la IP para verificar que haya conexion

Escaneo de Puertos
Iniciamos con un escaneo básico de puertos y encontramos los siguientes servicios abiertos:

![image](https://github.com/user-attachments/assets/3f91c7d4-234b-4e49-a9d8-9fcaf6f60ebf)

Enumeración de Directorios
Utilizamos gobuster para enumerar directorios en el puerto 80:

gobuster dir -u http://172.18.0.2/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -t 20 -x html,php,txt,php.bak

![image](https://github.com/user-attachments/assets/2361a103-d894-4f12-b8cf-09f2ff44cc11)

Resultado
Encontramos un archivo interesante:

/secret.php
Accedemos a " http://172.18.0.2/secret.php ", pero solo encontramos un mensaje que dice "HOLA MARIO". Sin más información útil, decidimos explorar otras opciones.

![image](https://github.com/user-attachments/assets/2f3034b3-d3b6-4340-9b76-267ee5e5a933)

Fuerza Bruta en SSH
Dado que encontramos un usuario potencial ("mario") en /secret.php, procedemos a un ataque de fuerza bruta en el puerto 22 (SSH) para descubrir la contraseña:

hydra  -l mario -P /home/kali/Downloads/rockyou.txt ssh://172.18.0.2

Credenciales encontradas:
Usuario: mario
Contraseña: chocolate

Acceso al Sistema
Usamos las credenciales obtenidas para iniciar sesión por SSH:
ssh mario@172.18.0.2

Escalada de Privilegios
Al verificar los privilegios de sudo con sudo -l, observamos lo siguiente:

![image](https://github.com/user-attachments/assets/dacc3017-5890-4356-ab6a-bc0bed7de500)

Esto indica que podemos usar vim como root. Para explotar este permiso, consultamos GTFOBins.

Comando para obtener acceso root:
sudo vim -c ':!/bin/sh'

Al ejecutar este comando, logramos escalar privilegios y obtener acceso root.

![image](https://github.com/user-attachments/assets/299a427e-5260-4673-9734-7f0c61668cef)


¡Y eso es todo, ya somos root!







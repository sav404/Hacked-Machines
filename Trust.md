#### INFORMACION DE LA MAQUINA.
#### Maquina Trust.
[LINK DE LA MAQUINA](https://mega.nz/file/UacxFKDR#G5KBHBt8ASB0lHPuttnaxKROAa40FMGrvBoIBf6ak0E)
#### Dificultad: Muy facil.

---

Enviamos un ping de un paquete para comprobar que la maquina esta activa y lista.
```Linux
Ping -c 1 172.17.0.2
```
![Captura de pantalla 2024-05-29 001656](https://github.com/sav404/Hacked-Machines/assets/169759579/e0daf810-4ccc-4a61-884b-46a39631509c)

Si entramos a la ip en el navegador podemos ver que en el puerto 80 hay un servidor apache2 configurado, el cual tiene la pagina por defecto que te entregan al activar el servidor.

---

Comenzamos el proceso con el reconocimiento del objetivo mediante el uso de la herramienta Nmap. Realizaremos un escaneo exhaustivo de todos los puertos disponibles, utilizando un escaneo de tipo SYN, junto con un conjunto de scripts que maximizarán la capacidad de reconocimiento, así como otro que nos proporcionará información sobre la versión de los servicios. Este escaneo se realizará enviando paquetes a una velocidad mínima de 5000 por segundo, sin realizar resolución DNS, y con un nivel de vervose triple en la salida para que los puertos abiertos se notifiquen progresivamente a medida que avanza el escaneo. Además, se incluirá un ping para verificar la conectividad previa.
```Linux
sudo nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn 172.17.0.2
```

El escaneo detecto que la maquina tiene el puerto 22 protocolo SSH y puerto 80 protocolo HTTP abiertos.
Resultado de el escaneo:
```Linux
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.57 ((Debian))

```
---

Vamos a hacer fuerza bruta a la url de la pagina para poder encontrar directorios ocultos utilizando la herramienta GoBuster y una lista common de palabras. añadimos que busque extensionés html y php, para encontrar mas directorios posibles en el servidor web.
```Linux
gobuster dir -u http://172.17.0.2 -w usr/share/wordlists/dirb/common.txt html,php
```

Resultado de fuerza bruta:
``` Linux
/index.html           Status: 200
/secret.php           Status: 200
```

Accedémos a secret.php y encontramos una pagina la cual dice que no puede ser hackeada.
![Captura de pantalla 2024-05-29 005453](https://github.com/sav404/Hacked-Machines/assets/169759579/49517ba4-d3b3-42d5-8bab-2aa524dde070)

---

Vamos a intentar un ataque de fuerza bruta al puerto 22 SSH, ya que vimos que está abierto. Usaremos Hydra, especificando `-l` para el nombre de usuario, que sabemos que es "Mario", y `-P` para la contraseña, usando el diccionario de contraseñas RockYou y 64 hilos de velocidad.
```Linux
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64

```
Resultado de la fuerza bruta.
![Captura de pantalla 2024-05-29 011225](https://github.com/sav404/Hacked-Machines/assets/169759579/d8a08fc8-caf0-4d21-9d14-6751d3d51d2f)


Credenciales obtenidas ya tenemos acceso a el usuario mario el cual su contraseña es: chocolate.

---

A continuación nos conectamos con el comando ssh y pasandole el nombre de usuario arrobado con la ip de la maquina victima u objetivo.
```Linux
ssh mario@172.17.0.2
```

Nos saldra este aviso para que veamos si estamos intentando conectarnos al servidor correcto y no nos hayamos equivocado para entrar a uno malicioso, sale siempre al conectarse mediante ssh a una maquina por primera vez, puedes poner que yes y listo.
![Captura de pantalla 2024-05-29 011029](https://github.com/sav404/Hacked-Machines/assets/169759579/7df6cad3-cb5e-4d81-a6a9-40db20697026)

A continuacion nos pide la contraseña la cual como ya sabemos es **Chocolate**
![Captura de pantalla 2024-05-29 011029](https://github.com/sav404/Hacked-Machines/assets/169759579/af1ccc43-65c7-4913-a446-8530e23aee6d)

---
Con el siguiente comando una vez dentro del usuario mario vamos a ejecutar como root el ide: vim.
```Linux
sudo -u root /usr/bin/vim
```

Ahora dentro del mismo vim presionamos control + z , lo cual nos abrira una pequeña seccion en la parte de abajo de todo, donde con doble punto :q podemos salir o escribri con :w etc, lo que mucha gente no sabe es que por ahi tambien podemos abusar para ejecutar comandos como por ejemplo abrirnos una bash que es lo que haremos a continuacion.
```Vim
:!/bin/bash
```
![Captura de pantalla 2024-05-29 011927](https://github.com/sav404/Hacked-Machines/assets/169759579/2b2546c1-f2fe-4ab0-8a14-880d43c0175a)

Gracias a eso, escalamos privilegios y obtuvimos el usuario con mayores privilegios root.
![Captura de pantalla 2024-05-29 012304](https://github.com/sav404/Hacked-Machines/assets/169759579/c46bedb5-a9da-4fea-a523-07c2889e9766)

---

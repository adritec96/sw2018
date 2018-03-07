# Practica 2: Clonación de la información de un sitio web

## Crear un tar con ficheros locales en un equipo remoto:

Para ello utilizamos el comando tar de a siguiente manera:
*tar -czvf archivo.tar ./carpeta/*

-c : indica a tar que cree un archivo.
-z : indica que use el compresor gzip
-f : indica a tar que el siguiente argumento es el nombre del fichero.tar
-v : indica a tar que muestre lo que va empaquetando

Para realizar esta tardea en remoto lo utilizaremos de esta manera:

*tar -czv ./carpeta/ | ssh usuario@ip_maquina_destino '  cat > archivo.tgz '*

![imagen tar remoto](https://github.com/adritec96/sw2018/blob/master/p2/tar_remoto.png)

En el comando tar no tenemos que especificar nombre de archivo, ya que la salida la vamos a redirigir con un pipeline hacia un ssh, el cual relizaa un con cat (con el flujo de salida del comando tar) y a su vez este flujo será rediriguido a un archivo.tar

## Uso de rsync para clonado de un directorio:

realizamos el comando *rsync -avz -e ssh ip_maquina_origen:/ruta/del/directorio/ /ruta/local/destino/*

![imagen copia rsync](https://github.com/adritec96/sw2018/blob/master/p2/capturas/rsync_otro.png)

podemos ver como se ha relizado una copia del archivo hola.html que estaba en la maquina1 (podemos comprobarlo con cat).

[IMFORMACION APLICACION](https://rsync.samba.org)


## Uso de ssh sin contraseña

Lo primero que tenemos que hacer es crear una clase privada y una clave publica de nuestro servidor que se va a conectar al "maestro" para ello realizamos lo siguiente:
![imagen generacion de claves](https://github.com/adritec96/sw2018/blob/master/p2/capturas/generacion_claves.png)

Después tenemos que añadir la clae publica al servidor origen, para ello desde el servidor destino ejecutamos este comando:
![imagen añadiendo clave publica](https://github.com/adritec96/sw2018/blob/master/p2/capturas/add_clave_publica.png)

Y con esto ya podremos conectarnos sin contraseña!

![imagen conexion sin password ](https://github.com/adritec96/sw2018/blob/master/p2/capturas/ssh_sin_pass.png)

Tambien podemos ejecutar un comando remoto de esta manera:
*ssh username@ip_maquina_origen comando -modificadores*
y obtendremos por nuestra salida de pantalla el comando.



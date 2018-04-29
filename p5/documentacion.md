## Practica5: Replicación de bases de datos MySQL


# Primer paso: Instalar mysql, crear las tablas de ejemplo y añadirle algunos datos.


Lo primero que tenemos que hacer es instalar mysql-server para ello, usaremos:

*sudo apt-get update*

*sudo apt-get install mysql-server*

también usaremos este comando para realizar una pequeña configuración inicial de seguridad de mysql:

*mysql_secure_installation*


Una vez tenemos instalado mysql solamente tenemos que iniciar secion con:

*mysql -u root -p*

y crear las tablas de la siguiente manera:

![imagen comandos1](https://github.com/adritec96/sw2018/blob/master/p5/capturas/comandos1.png)

*create database contactos;*   **Creamos la base de datos contactos**

*use contactos;* 	**Realizamos la asignacion de la base de datos donde vamos a trabajar**

*show tables;*		**Mostramos las tablas que hay en la base de datos seleccionada**


como podemos observar ahora tenemos la bd creada pero sin ninguna tabla, vamos a crear una tabla y añadir algunos datos:

![imagen comandos2](https://github.com/adritec96/sw2018/blob/master/p5/capturas/comandos2.png)

*create tables datos(nombre varchar(100), tlf int );*   **Creamos la tablas espeficicando el tipo de dato por columna**
*insert into datos(nombre,tlf) values("pepe",999999)*   **Insertamos un elemento con nombre pepe y telefono 999999**

Ahora cuendo realizamos un *show tables* podemos observar como ya nos muestra la tabla creada.


para comprobar que todo se ha echo correctamente podemos hacer estas consultas:

![imagen consulta1](https://github.com/adritec96/sw2018/blob/master/p5/capturas/consulta1.png)

*select \* from contactos;*		**Realizar consulta de los datos de una tabla**

*describe datos;*				**Realizar consulta de los atributos de una tabla** 


# Segundo Paso: Realizar backup local, transferir copia y restaurar en 2 maquina:

Para realizar el backup local vamos a utilizar mysqldump pero antes de nada, tenemos que bloquear las tablas para que durante el backup no se esté escirbiendo en las mismas tablas, para ello utilizamos *FLUSH TABLES WITH READ LOCK;* desde la consola de mysql:

![imagen desactivar](https://github.com/adritec96/sw2018/blob/master/p5/capturas/desactivar.png)

y ya podemos usar el comando de mysqldump: **NOTA:** guardar en un sitio con permisos, no en root, sino después para pasarlo por ssh o otra forma tendrás problemas.


![imagen ejecucion1](https://github.com/adritec96/sw2018/blob/master/p5/capturas/ejecucion1.png)


Para pasar el archivo que acabamos de crear, utilizaremos scp, para ello nos diriguimos a la maquina destino, donde queremos que se copie el archivo y realizamos el siguiente comando:

![imagen scp1](https://github.com/adritec96/sw2018/blob/master/p5/capturas/scp1.png)

podemos ver que ya nos realizó la copia de nuestro archivo *contactos.sql*. Ahora para realizar la restauración en esta maquina (debe tener mysql-server instalado ya) solamente tenemos que realizar la creación de la base de datos antes de hacer el volcado de información.

![imagen creardatabase](https://github.com/adritec96/sw2018/blob/master/p5/capturas/creardatabase.png)

una vez creada la bd en la 2 maquina, realizamos la restauración:

![imagen importacion](https://github.com/adritec96/sw2018/blob/master/p5/capturas/importacion.png)

para comprobar que todo se ha importado correctamente podemos acceder a la consola de mysql y realizar las consultas aprendidas en la parte1:

![imagen ya_copiados](https://github.com/adritec96/sw2018/blob/master/p5/capturas/ya_copiados.png)


# Tercer Paso: Realizar la Replicacion automaticamente usando maestro-esclavo

Lo primero que tenemos que hacer es modificar las configuraciones de las dos maquinas. Comenzaremos por la MAESTRO(maquina1):

*sudo nano /etc/mysql/mysql.conf.d/mysql.cnf*

tenemos que editar los siguientes parametros:

*bind-address 127.0.0.1*  **comentar esta linea**

*log_error = [ruta]*      **especificar ruta de log (se puede dejar por defecto)**

*server-id = 1* 			**descomentar esta linea y poner  = 1**

*log_bin = [ruta]* 			**especificar ruta del registro de actualizaciones**

después solamente tenemos que reiniciar el servicio con */etc/init.d/mysql restart*

En la configuración del ESCLAVO(maquina2) y relizar la misma configuracion pero en id pondremos = 2.


Ahora volvemos a ir a la MAESTRO(maquina1) y abrimos una consola mysql para crear el usuario de la replicación y darle permisos, para ello realizaremos los siguientes comandos:

![imagen pv](https://github.com/adritec96/sw2018/blob/master/p5/capturas/pv.png)

A continuación usamos el siguiente comando para ver la informacion de la maquina maestro y poder configurar la maquina esclavo. (copiar datos en un papel)

![imagen status](https://github.com/adritec96/sw2018/blob/master/p5/capturas/status.png)

Una ve creado el usuario con sus permisos correspondientes, y con los datos del servidor maestro, podemos realizar la configuracion en la maquina ESCLAVO(maquina2). Para ello ejecutaremos en una consola de mysql la siguiente sentencia:

*CHANGE MASTER TO MASTER_HOST='192.168.31.200',*
*MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',*
*MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=501,*
*MASTER_PORT=3306;*

**NOTA:** Comprobar que los puertos 3306 no están bloqueados de la practica anterior.

Por ultimo relizaremos tambien un *START SLAVE*. En la maquina MAESTRO(maquina1) en una consola de mysql tenemos que ejecutar *UNLOCK TABLES* ya que habiamos bloqueado las tablas anteriormente.


Y ya pondremos comprobar que la sincronización se realiza correctamente:

![imagen sincronizados](https://github.com/adritec96/sw2018/blob/master/p5/capturas/sincronizados.png)

Podemos observar como en la parte superior es la maquina maestro y si realizamos una inserccion de un elemento nuevo, y en la maqina inferior ( la esclavo) le hacemos la consulta, se han replicado correctamente.

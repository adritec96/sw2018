# Practica 3:	Balanceo de carga

## Instalar y Configurar el balanceador de carga nginx:


Para ello utilizamos el comando apt-get install para instalarlo:

*apt-get install nginx*

**IMPORTANTE** Tener el SO totalmente actualizado  y la configuración de red correcta, con conexion entre las maqinas ( 192.168.56.103 será mi balanceador.)

Una vez lo tenemos instalado, tenemos que arracncarlo utilizando el comando:

*sudo systemctl strat nginx*

Una vez tenemos el servicio corriendo tenemos que configurarlo, para ello crearemos un archivo en:

*sudo nano /etc/nginx/conf.d/default.conf*

dentro de este archivo realizaremos la siguiente configuracion:

![imagen conf1](https://github.com/adritec96/sw2018/blob/master/p3/capturas/conf1.png)

Podemos añadir

	upstream apaches { 

	ip_hash;

	server 172.168.56.130 weight=1 max_fails=3  fails_timeout = 30s;

	server 172.168.56.131 weight=2 max_fails=3 fails_timeout = 30s;

	server 172.168.56.132 .............  down;

	keepalive 3;

	\}

	**weight:** Modificamos la prioridad de reparto de peticiones ( cuando mas grande mas peticiones).
	**max_fails:** Especifica un número de intentos de comunicación erróneos en "fail_timeout" segundos para considerar al servidor no operativo (por defecto es 1, un valor de 0 lo desactivaría)
	**fails_timeout:** indica el tiempo en el que deben ocurrir "max_fails" intentos fallidos de conexión para considerar al servidor no operativo. Por defecto es 10 segundos
	**ip_hash:** Configuramos que cada ip se redirige a un backend. (Persistencia de Sesiones )
	**keepalive:** Configuramos que se realizen conexiones con cookies para mantener la identificacion.
	**down:** Configura como servidor caido y no manda peticiones hacia el.


Después tenemos que hacer una segunda configuración si tenemos una de las nuevas versiones de nginx, para ello accedemos a: 
*sudo nano /etc/nginx/nginx.conf*

Dentro de este archivo lo que tenemso que hacer es comentar una linea para que el servidor nginx no actue como un servidor web. (ultima linea de la imagen)

![imagen conf2](https://github.com/adritec96/sw2018/blob/master/p3/capturas/conf2.png)



Una vez esto, volvemos a realizar el reinicio del servicio nginx y ya tendremos todo preparado. Ahora realizaremos la prueba de que se esta realizando el balanceo de carga, para ello, desde la maquina anfitrion vamos a acceder a nuestras webs, para ello accedemos a la ip de nuestro balanceador ( http://192.168.56.103/hola.html ) y podemos comprobar que en cada recarga de la web, se accede a un servidor distinto, tal y como lo habiamos configurado.

**IMPORTANTE:** En la practica anterior tenemos una copia de la m1 y la m2 programada, para comprobar que se está haciendo el balanceo, desactivaremos esta sincronizacion y haremos que cada web sea distinta.


![test1](https://github.com/adritec96/sw2018/blob/master/p3/capturas/test1.png)



![test2](https://github.com/adritec96/sw2018/blob/master/p3/capturas/test2.png)



## Instalar y Configurar el balanceador de carga Haproxy:

Lo primero que tenemos que hacer es instalar y configurar una maquina limpia (yo he creado un clon de la maquina que usé para nginx y lo he desistalado ), una vez tengamos la maquina limpia, podemos proceder a instalar haproxy con el siguiente comando:
*sudo apt-get install haproxy*
La configuración por defecto no nos sirve, tenemos que modificar el archivo de configuracion    */etc/haproxy/haproxy.cfg*

![imagen conf2](https://github.com/adritec96/sw2018/blob/master/p3/capturas/conf3.png)

De esta manera, hemos configurado nuestro haproxy para que escuche cualquier ip por el puesto 80 y que esas peticiones las mande a los servidores backend los cuales son nuestros apaches con un limite de 32 conexiones por servidor.

Para lanzar el servicio solo tenemos que ejecutar *sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg* Ahora realizaremos las mismas pruebas, utilizando el navegador y usando solo la ip de nuestra maquina haproxy:


![test1](https://github.com/adritec96/sw2018/blob/master/p3/capturas/test2.png)



![test2](https://github.com/adritec96/sw2018/blob/master/p3/capturas/test1.png)

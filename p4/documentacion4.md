# Practica 4:	Asegurar Granja Web

## Generar un certificado autofirmado


Para ello utilizamos la siguiente serie de comandos:

![imagen cert1](https://github.com/adritec96/sw2018/blob/master/p4/capturas/cert1.png)

después ejecutamos

*openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt*

el cual ya nos generará los certificados.

![imagen cert2](https://github.com/adritec96/sw2018/blob/master/p4/capturas/cert2.png)

ahora lo que tenemos que editar es la configuracion de nuestro sitio con apache, para ello editaremos el siguiente fichero */etc/apache2/sites-available/default-ssl* y añadineremos las dos lineas de SSLCertificateFile.

![imagen cert3](https://github.com/adritec96/sw2018/blob/master/p4/capturas/cert3.png)

Y por ultimo activamos el default-ssl y reinicinamos el apache con:

*a2ensite default-ssl*

*service apache2 reload*


Al ingresar en la pagina con nuestro navegador utilizando *https://IP/hola.html* podemos observar que el navegador nos avisa que es un certificado autofirmado. y si lo aceptamos (nos fiamos de nosotros mismos..) podremos acceder a la web por https.

![imagen test1](https://github.com/adritec96/sw2018/blob/master/p4/capturas/test1.png)

![imagen test2](https://github.com/adritec96/sw2018/blob/master/p4/capturas/test2.png)

A continuación tenemos que copiar el certificado que hemos creado a la maquina2 y al balanceador de carga. para ello podemos utilizar por ejemplo:

![imagen copia](https://github.com/adritec96/sw2018/blob/master/p4/capturas/copia.png)


En la maquina 2 tenemos que repetir los comandos(1-8) para activar ssl, crear la carpeta ssl y mover los certificados que hemos copiado a esta maquina. tambien reiniciamos la maquina.

![imagen history](https://github.com/adritec96/sw2018/blob/master/p4/capturas/history.png)

Para el balanceador es mas dificil, ya que no tiene un apache instalado, una vez tenemos los certificados de la maquina1 tenemos que configurar ngixn para que funcione tanto por http y https.


![imagen confssl](https://github.com/adritec96/sw2018/blob/master/p4/capturas/confssl.png)

de esta manera ya podemos acceder al balanceador por https y nos realizara el balanceo.

![imagen testhttps](https://github.com/adritec96/sw2018/blob/master/p4/capturas/testhttps.png)


##Configurar un cortafuegos
info: He estado intentado realizar la configuración de poner el cortafuegos por delante del balanceador pero he tenido una serie de errores que no me han dejado realizarlo correctamente, a petición de Pedro, no he dedicado mas tiempo ya que parece que es mas complicado de lo que aparentaba.. entonces he realizado la configuracion del cortafuegos externo a una de las maquinas servidoras.

Para ello creamos una nueva maquina con la ip 192.168.56.104

y hemos realizado un scrip para realizar la configuracion del iptables.



![imagen iptables1](https://github.com/adritec96/sw2018/blob/master/p4/capturas/iptables1.png)

![imagen iptables2](https://github.com/adritec96/sw2018/blob/master/p4/capturas/iptables2.png)


despues solamente tenemos que ejecutar el scrip y podemos ver gracias al comando *iptables -L -n -v* que se ha realizado correctamente la configuración.

![imagen iptables3](https://github.com/adritec96/sw2018/blob/master/p4/capturas/iptables3.png)
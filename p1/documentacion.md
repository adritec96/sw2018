# Practica 1: Instalación de Maquinas virtuales

## Configuración red:
He realizado la siguiente configuracion red:

Dos tarjetas de red en cada maquina virtual, una configurada en Solo-anfitrión para la comunicación ente ellas y la otra en Nat para que tengan acceso a internet y podamos realizar las inatalaciones de las siguientes practicas.

Adaptador1[enp0s3]: Solo-anfitrion (red-interna incluyendo la maquina anfitrión)
Adaptador2[enp0s6]: Nat

La red creada en Solo-anfitrión hemos creado la red 192.168.56.0
y hemos configurado la ip 192.168.56.101 a la maquina1:
![imagen](https://github.com/adritec96/sw2018/blob/master/p1/ifconfig_m1.png)


y la ip 192.168.56.102 a la maquina2:
![imagen](https://github.com/adritec96/sw2018/blob/master/p1/ifconfig_m2.png)


**IMPORTATNTE:** El archivo de configuracion de red lo hemos configurado del siguiente modo:
![imagen](https://github.com/adritec96/sw2018/blob/master/p1/interfaces.png)

Ya que Ubuntu selecciona una de los dos adaptadores como principal (el primero que se lee dentro de este archivo) y si ponemos primero el Solo-anfitrión teniamos el problema de que no tenia la conexión a internet.

**IMPORTATNTE2:** tenemos que añadir esta linea al final del adaptador solo-anfitrion para que nunca se ponga ese adaptador como principal:
*post-up route del default dev $IFACE*

Con todo esto ya tenemos conexion entre la mv y el anfitrion y internet:
![imagen](https://github.com/adritec96/sw2018/blob/master/p1/conexion_total.png)


## Test de Apache
Hecreado un archivo html simple en cada mauqina y he instalado curl en cada una de ellas y lo he utilizado para descargar el archivo html de la otra.

![imagen](https://github.com/adritec96/sw2018/blob/master/p1/test_apache.png)

## Test ssh
También instalamos ssh durante la instalación de ubuntu y lo testeamos la conexion entre las dos maquinas:
![imagen](https://github.com/adritec96/sw2018/blob/master/p1/test_ssh.png)
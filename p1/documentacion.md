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

Ya que Ubuntu selecciona una de los dos adaptadores como principal (el primeor que se lee dentro de este archivo) y si poniamos primero el Solo-anfitrión teniamos el problema de que no tenia la conexion a internet.



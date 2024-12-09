## ¿Por que Hacer un Ping?
Hacer un ping a una ip nos sirve para hacer un diagnostico de este, para saber si recibe nuestros paquetes entre otras cosas que tienen importancia como la velocidad de respuesta, además de que podemos saber que tipo de sistema operativo tiene la ip a la cual estamos consultado. 

## Datos importantes dentro de un ping: 
dentro de los datos importantes que nos proporciona un ping es el TTL, por que gracias a esto podemos identificar si estamos frente a una maquina Linux o Windows, ya que las maquinas Linux tienen un TTL <= 64 y las maquinas Windows tienen un TTL > 64 && <= 128. 

## Ejemplos de uso de Ping

```
1.- ping [ip] = aquí se hará un envio y recepcion de paquetes constante haste que el usuario determine acabar el comando

2.- ping -c 1 [ip] = aquí el -c cumple la funcion de limitar la cantidad de respuestas que se recibiran indicandolo con el número siguiente al parametro
```

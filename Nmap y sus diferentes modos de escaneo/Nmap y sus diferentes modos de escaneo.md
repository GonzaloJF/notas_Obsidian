## Mis apuntes:

Para poder vulnerar algún dispositivos tenemos que reconocer cuales son los puertos que están abiertos por lo que tenemos esta herramienta llamada nmap, algo que tenemos que tener en cuenta es que hay 65535 puertos.

podemos hacer un route -n para ver las ip de nuestra network. 
![[Pasted image 20240204223107.png]]
ese Gateway es el que nos importa
por lo que ahora podemos hacer el primer nmap: 
```
$ nmap -p- [ip]

este comando sirve para escanear todo el rango de puertos de una red
```
![[Pasted image 20240204225339.png]]
aquí podemos ver que nos ponen los puertos encontrados, el estado del puerto, puede estar abierto o filtrado, que este filtrado quiere decir que no necesariamente va a responder a los paquetes icmp.
Una forma de poder hacer que pruebe solo los puertos más comunes es agregando el parámetro --top-ports de la siguiente forma: 
```
$ nmap --top-ports 500 --open 192.168.1.1

el filtro --open es para que solo nos muestre los puertos abiertos. 
```
![[Pasted image 20240204225836.png]]
una de los parámetros que podemos hacer para ver el escaneo mientras se realiza es el parametro -v de verbose: 
```
$ nmap -p- --open 192.168.1.1 -v 
```
![[Pasted image 20240204233157.png]]
otro parámetro que es muy útil para agilizar el escaneo es el parámetro -n, este sirve para poder saltarte la resolución DNS:
![[Pasted image 20240204234058.png]]
Para poder agilizar aun más el escaneo, tenemos un parámetro para controlar el tiempo de escaneo, es decir que se puede ir desde 0 que es muy lento hasta 5 que es muy rapido, este parametro es el -T, decir que puede ir de  -T0 a -T5:
![[Pasted image 20240205112305.png]]
Para poder hacer un scaneo de los puertos por el protocolo TCP, lo podemos hacer con el parametro -sT, para terlo en cuenta lo que hace por detras este tipo de escaneo ocupa el modelo de escaneo three way handshake, esta se representa como:![[Pasted image 20240205113358.png]]
se envía un paquete SYN, y tiene 2 posibilidades de respuesta un RST que es un reset packet, es decir que el puerto esta cerrado, en cambio si hay una respuesta SYN/ACK es decir que el puerto esta abierto, y nosotros mandaríamos de vuelta un paquete ACK(established), que significa que se puede establecer una conexión.

![[Pasted image 20240205113755.png]]
podemos ver en la imagen los puertos que se reportan son los puertos ocupados tcp.
podemos ver lo que se hacer  por detrás gracias a la herramienta tcpdump: 
```
$ tcpdump -i ins33 -w Captura.cap -v
```
![[Pasted image 20240205142800.png]]
![[Pasted image 20240205142815.png]]
ahora con wireshark podemos inspeccionar la captura: 
```
$ wireshark Captura.cap &>/dev/null & disown
```
podemos hacer un filtro dentro de wireshark:![[Pasted image 20240205145250.png]]
otro parámetro importante dentro del escaneo es el parámetro -Pn que es para que no haga reconocimiento de hosts, lo que hace este parametro es que asimila que todos los host están levantados para poder ser escaneados. 
![[Pasted image 20240205151854.png]]
Bueno ya vimos que se por defecto o con el parámetro -sT podemos ver los puertos a través del protocolo TCP, para hacer reconocimiento de los puertos UDP, tenemos el parámetro -sU
![[Pasted image 20240205152708.png]]
Podemos también  descubrimiento de dispositivos dentro de la red con el parámetro -sn aplicándolo de la siguiente forma: 
![[Pasted image 20240205153234.png]]
ahora entramos en el las funcionalidades más importantes que tiene nmap, ya que con el parámetro -sV podemos ver la versión y el servicio que esta corriendo este puerto: 
![[Pasted image 20240205154420.png]].


----
## Datos de la Clase: 

**Nmap** es una herramienta de **escaneo de red** gratuita y de código abierto que se utiliza en pruebas de penetración (pentesting) para explorar y auditar redes y sistemas informáticos.

Con Nmap, los profesionales de seguridad pueden identificar los **hosts** conectados a una red, los **servicios** que se están ejecutando en ellos y las **vulnerabilidades** que podrían ser explotadas por un atacante. La herramienta es capaz de detectar una amplia gama de dispositivos, incluyendo enrutadores, servidores web, impresoras, cámaras IP, sistemas operativos y otros dispositivos conectados a una red.

Asimismo, esta herramienta posee una variedad de funciones y características avanzadas que permiten a los profesionales de seguridad adaptar la misma a sus necesidades específicas. Estas incluyen técnicas de escaneo agresivas, capacidades de scripting personalizadas, y un conjunto de herramientas auxiliares que pueden ser utilizadas para obtener información adicional sobre los hosts objetivo.
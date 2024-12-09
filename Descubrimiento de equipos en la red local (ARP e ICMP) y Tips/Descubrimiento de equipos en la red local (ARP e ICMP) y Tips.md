Para descubrir host dentro de la red podemos jugar con algunas herramientas, o haciendo un bash scripting: 
En este caso uno de las herramientas que más se utilizan es arp-scan: 
```
$ arp-scan -I eth0 --localnet --ignoredups

-I = interfaz de red.
--localnet = hacer un escaneo a la red local.
--ignoredups = no muestra ips duplicadas.
```
Otra de las herramientas de las cuales se pueden utilizar para hacer un reconocimiento de dispositivos  es netdiscover: 
```
$ netdiscover -i eth0
```
Una de las formas manuales es hacer un script con ping de la siguiente forma: 
![[Pasted image 20240206195510.png]]
Una herramienta más es masscan, en la cual se puede hacer lo siguiente: 
```
$ masscan -p22,80,8080,etc -Pn 192.168.1.0/24 --rate=1000

-p puedes poner los puertos que se necesiten.
-Pn para no hacer host discover.
--rate sirve para poder darle una velcidad de envio de paquetes.
```

----
## Info de la clase: 

El descubrimiento de equipos en la red local es una tarea fundamental en la gestión de redes y en las pruebas de seguridad. Existen diferentes herramientas y técnicas para realizar esta tarea, que van desde el escaneo de puertos hasta el análisis de tráfico de red.

En esta clase, nos enfocaremos en las técnicas de descubrimiento de equipos basadas en los protocolos **ARP** e **ICMP**. Además, se presentarán diferentes herramientas que pueden ser útiles para esta tarea, como **Nmap**, **netdiscover**, **arp-scan** y **masscan**.

Entre los modos de escaneo que se explican en la clase, se encuentra el uso del parámetro ‘**-sn**‘ de Nmap, que permite realizar un escaneo de hosts sin realizar el escaneo de puertos. También se presentan las herramientas netdiscover, arp-scan y masscan, que utilizan el protocolo ARP para descubrir hosts en la red.

Cada herramienta tiene sus propias ventajas y limitaciones. Por ejemplo, **netdiscover** es una herramienta simple y fácil de usar, pero puede ser menos precisa que **arp-scan** o **masscan**. Por otro lado, **arp-scan** y **masscan** son herramientas más potentes, capaces de descubrir hosts más rápido y en redes más grandes, pero también son más complejas y pueden requerir más recursos.

En definitiva, el descubrimiento de equipos en la red local es una tarea fundamental para cualquier administrador de redes o profesional de seguridad de la información. Con las técnicas y herramientas adecuadas, es posible realizar esta tarea de manera efectiva y eficiente.

[](https://hack4u.io/cursos/introduccion-al-hacking/14714)
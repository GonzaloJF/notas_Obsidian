## Fase de reconocimiento

Primero haremos el escaneo de la red con arp-scan:
![[Pasted image 20240819224520.png]]
vemos la existencia de 3 ips en la red como no reconoce quien esta ocupando cada ip entonces tendremos que buscar en el navegador.
![[Pasted image 20240819224717.png]]
Si hacemos un ping podemos ver que nos responde la traza ICMP 
![[Pasted image 20240819224636.png]]
Ahora podemos empezar con la fase de escaneo de puertos
## Fase de escaneo de puertos:
primero lanzaremos nmap para reconocer los puertos que estan abiertos:
![[Pasted image 20240819235437.png]]
para luego escanear los puertos uno por uno:
![[Pasted image 20240819235510.png]]
Lance algunas reconocimientos de extensiones de archivos en gobuster:
![[Pasted image 20240820131832.png]]
ese es el primero para ver si hay algún archivo o pagina o directorio a la cual podemos entrar. 
![[Pasted image 20240820131930.png]]
en esta es para ver que directorios estaban activos en la búsqueda pero podemos ver los típicos de icons, manual o server-status.
también probamos buscando subdominios pero no encontramos nada.
![[Pasted image 20240820132138.png]]
por lo que ahora intentaremos es enumerar el contenedor smb que esta en la maquina, de la siguiente manera: 
![[Pasted image 20240820132232.png]]
podemos ver que tenemos algunos nombres como anonymous que nos podria interesar y también un helios que puede ser algun usuario que contiene la maquina. 
![[Pasted image 20240820132331.png]]
si ocupamos smbmap podemos ver que tenemos solo acceso a lectura en el directorio anonymous y no tenemos permisos en los demas directorios de la aplicacion smb
![[Pasted image 20240820151540.png]]
Nos metemos en el directorio anonymous y podemos ver un archivo txt, así que lo sacaremos con el comando get y ahora podemos ver el archivo en nuestra maquina: 
![[Pasted image 20240820151829.png]]
Nos dice que deberían terminar de ocupar las password 'epidioko','qwerty' o 'baseball'.
por lo que tenemos potenciales contraseñas para poder encontrar una vulnerabilidad ya que tenemos ssh en el puerto 22, y tambien podemos probar si helios es un usuario
podemos intentar con fuerza bruta poder entrar a este directorio. 
ahora podemos intentar entrar al directorio helios con smbclient utilizando los siguientes comandos: 
![[Pasted image 20240820153527.png]]
podemos ver en la imagen que guardamos todas las posibles contraseñas, intentamos una por una y pudimos encontrar que el usuario helios ocupa la contraseña 'qwerty'
ahora podemos revisar la el directorio tranquilamente. 
![[Pasted image 20240820153756.png]]
encontramos lo siguiente en los contenidos de ambos archivos. 
necesitamos agregar la siguiente ip dentro del archivo /etc/hosts
![[Pasted image 20240820155100.png]]
si ocupando wpscan podemos ver lo siguiente: 
![[Pasted image 20240821174351.png]]
![[Pasted image 20240821174432.png]]
podemos ver que no encuentra plugins vulnerables, que se puede escanear con api de wpscan y podriamos tener algo distinto pero en este caso podriamos hacer un simple curl para ver si tenemos acceso a wp-content:
![[Pasted image 20240821175708.png]]
podemos ver 2 plugins en este caso que es site-editor y mail-masta, si buscamos en google, encontramos la siguiente pagina de exploit-db: 
https://www.exploit-db.com/exploits/40290
dentro de esto podemos ver el que si ejecutamos este exploit en el navegador tenemos la oportunidad de ver lo siguiente: 
![[Pasted image 20240821184101.png]]
con esto podemos demostrar que hay un usuario helios dentro de la maquina 
ahora si revisamos los puertos abiertos tenemos el puerto 25, si buscamos en google que es el puerto 25 podemos encontrar lo siguiente: 
https://www.mailjet.com/es/blog/emailing/servidor-smtp/
mandamos un mail de la siguiente forma: 
![[Pasted image 20240821195635.png]]
donde le mandamos un srcipt en php para poder ejecutar comandos de la cmd. como el siguiente: 
![[Pasted image 20240821195721.png]]
donde nos entrega el id, si hacemos el control+u podemos verlo mejor:
![[Pasted image 20240821195801.png]]
mandamos el one liner para tomar acceso a la consola:
![[Pasted image 20240821195928.png]]
consiguiendolo :
![[Pasted image 20240821195948.png]]
y ahora podemos hacer el tratamiento de la tty:
![[Pasted image 20240821200021.png]]
ahora tenemos que hacer el tratamiento de la terminal con los siguientes comandos. 
script /dev/null -c bash
ctrl + z
 stty raw -echo; fg 
reset xterm 
export TERM=xterm 
export SHELL=bash
stty rows 52 columns 248
![[Pasted image 20240821200518.png]]
luego empezamos a buscar las flags correspondientes.
si hacemos un find / -perm /4000 2>/dev/null
tenemos el siguiente archivo: 
![[Pasted image 20240821201106.png]]
donde esta statuscheck, que esta siendo ejecutado como root. 
![[Pasted image 20240821201132.png]]
esto es literalmente un curl, lo cual podemos hacer un path hijacking.
de la siguiente forma: 
![[Pasted image 20240821201546.png]]
vamos al directorio tmp, hacemos un echo de chmod u+s /bin/bash > curl, es decir que al ejecutar el curl le damos los permisos completos a la bash le cambiamos los permisos al curl para que todos puedan ejecutarlo, exportamos en el PATH el directorio actual más el los directorios anteriores, ejecutamos el estatuscheck y terminamos con bash -p para tener acceso como root. 
![[Pasted image 20240821201821.png]]


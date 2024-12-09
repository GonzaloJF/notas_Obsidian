Empezamos con la maquina DarkHole1: 
## Escaneo de red: 
Arp-scan: 
![[Pasted image 20240206234048.png]]
![[Pasted image 20240206235800.png]]
![[Pasted image 20240206235908.png]]
con esto tenemos 3 formas de enumerar la red, es decir que estas son 3 herramientas en las cuales podemos ver los dispositivos que esta en la red.
## Escaneo de puertos: 
primero con nmap podemos hacer  un self scan para ir reconociendo los puertos abiertos, el comando completo es `nmap -p- --open -sS -vvv -n -Pn --min-rate 5000 <ip> -oG allPorts`: 
![[Pasted image 20240207012321.png]]
la salida -oG es para generar un tipo de output con la información, que es de tipo genérico.
![[Pasted image 20240207014840.png]]
con este reconocimiento podemos ver en un principio que los puertos 22 y 80 son los que están abiertos. 
![[Pasted image 20240207114433.png]]
Con gobustes hicimos una enumeración de contenido vemos que hay direcciones upload y config, si le aplicamos un filtro de tipo con -x: 
![[Pasted image 20240207115408.png]]
ahora entramos a login.php:
![[Pasted image 20240207115450.png]]
probamos haciendo una sql injecction por ejemplo admin' or '1'='1 -- -, pero no sirve por lo que podemos ver que hay un boton que dice Sign up now. 
![[Pasted image 20240207121719.png]]
creamos un usuario en este caso fue gonza, gonza@gonza.com, Password123, nos logeamos dentro de la pagina y podemos ver lo siguiente: 

Tenemos el siguiente url, en el caso de que cambiemos el parámetro id http://192.168.1.82/dashboard.php?id=2, no mandara un error: 
![[Pasted image 20240207122640.png]]
vamos a abrir burpsuite y vamos a tratar de cambiar la contraseña de nuestro usuario:
![[Pasted image 20240207124343.png]]
vemos que la contraseña y el id viajan en la petición por lo que podriamos intentar cambiar el id por el numero 1. 
![[Pasted image 20240207124514.png]]
vemos que tenemos acceso a la cuenta admin: 
![[Pasted image 20240207124609.png]]
tenemos un espacio llamado upload, por lo que podriamos intentar hacer un script en php para poder subirlo: 
si intentamos subir un archivo .php nos sale el siguiente error: 
![[Pasted image 20240207125111.png]]
vamos a interceptar la petición de subida de archivo: ![[Pasted image 20240207130306.png]]
luego de esto podemos hacer un ctrl + i, para enviar la petición al intruder, donde procederemos a hacer un ataque de cambio de tipo de archivo.
![[Pasted image 20240207132111.png]]
estas son las extensiones que probaremos: 
![[Pasted image 20240207132205.png]]
Al terminar el ataque podemos ver lo siguiente todos los tipos de extensiones fueron permitidos ya que todos son status 200. 
![[Pasted image 20240207132538.png]]
Una vez subido el archivo podemos ir a la dirección uploads:
ahora podemos probar en la misma url el comando en de php: 
![[Pasted image 20240207153757.png]]
el payloads que se cargo en este caso fue el siguiente: 
![[Pasted image 20240207153850.png]]
por lo que si en la url le agregamos un ``?cmd=<comando de linux>``, podremos ver interpretado nuestro comando: 
por lo tanto ahora podemos ocupar netcat para poder conectarnos a la maquina atacante utilizando un oneliner de reverse shell: 
netcat para entrar en modo escucha: 
![[Pasted image 20240207154102.png]]
one liner:
![[Pasted image 20240207154520.png]]
una vez ya dentro de la maquina como el usuario www-data podemos hacer un ls -la
![[Pasted image 20240207161537.png]]
lo hacemos dentro de la carpeta /home/john que es un directorio que tiene un usuario: 
este archivo toto es suid:
![[Pasted image 20240207161735.png]]
al ejecutar el archivo toto podemos ver que  hay un usuario llamado john:
![[Pasted image 20240207161945.png]]
con esto podemos ver tambien de que el id desde el directorio, no desde la consola por lo que si vamos al directorio /tmp podemos crear un id ![[Pasted image 20240207165046.png]]
antes de ejecutarlo debemos hacer un chmod 755 id para poder darle los permisos de ejecución y despues agregar el directorio tmp al PATH: 
![[Pasted image 20240207165737.png]]
Entonces ahora ejecutamos el archivo toto.
![[Pasted image 20240207165854.png]]
ganamos los privilegios de john.
primera flag user.txt
![[Pasted image 20240207165931.png]]
ahora podemos ver con ls -la que tenemos un archivo password: 
![[Pasted image 20240207170104.png]]
intentamos ingresar como root con la contraseña root123,
![[Pasted image 20240207170216.png]]
y no se puedo y tampoco con sudo su.
hay un archivo python vacío, podríamos ver si nos acepta un script para darle permisos de suid a la bash:
![[Pasted image 20240207170914.png]]
![[Pasted image 20240207173224.png]]
en este caso ocupando sudo /usr/bin/python3 y el directorio del archivo ya que tenemos la contraseña root123.
luego de ejecutar el script podemos ver que quedamos como root
![[Pasted image 20240207173311.png]]
como 
![[Pasted image 20240207174523.png]]

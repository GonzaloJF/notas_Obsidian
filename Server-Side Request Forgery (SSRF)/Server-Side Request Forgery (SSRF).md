## Mis apuntes: 

Para esta clase veremos 2 ejemplos, el primero es con el contexto de producción dentro de una maquina donde esta la pagina en la cual intentaremos vulnerar y otro dentro de la misma maquina pero que dentro de otro puerto intentaremos hacer conexión y ver lo que se esta trabajando en preproducción. 

Vamos a proceder a configurar el laboratorio: 
primero empezaremos a hacer 
![[Pasted image 20240304142500.png]]
para luego hacer lo siguiente: 
![[Pasted image 20240304152006.png]]
hacemos un docker run de la imagen anterior, luego se lanza una bash para poder hacer las configuraciones correspondientes: 
![[Pasted image 20240304152426.png]]
hacemos un apt update primero antes de instalar las demás herramientas: 
![[Pasted image 20240304152513.png]]
luego hacemos la instalación nano apache2 php python3
instalamos tambien la herramienta lsof para poder ver si hay algun servicio en los puertos que queremos ocupar: 
![[Pasted image 20240304153022.png]]
en este caso no hay ninguno: 
![[Pasted image 20240304155728.png]]
bueno ahora podemos ver que el puerto 80 esta ocupado con el servicio apache2.
![[Pasted image 20240304175249.png]]
con esto podemos entrar a la ruta /var/www/html, eliminamos el index.html, hacemos un script utility.php, el cual nos hace el filtrado por el parametro url: 
![[Pasted image 20240304175415.png]]
ahi podemos ver el script dentro de la maquina.
![[Pasted image 20240304182959.png]]
ahora podemos ver que estamos listando el sitio google.cl, pero no nos muestra nada, por que tenemos que hacer una búsqueda: 
```
find / -name php.ini 2>/dev/null
```
![[Pasted image 20240304184339.png]]
el archvo que buscamos es php.ini, lo tenemos que modificar dentro del archivo es un parametro que se llama allow_url_include que viene por defecto en Off, hay que cambiarlo a On.
si recargamos veremos lo siguiente: 
![[Pasted image 20240304190159.png]]
ahora haremos un recurso login dentro del sistema, sacado de la siguiente pagina: 
https://www.javatpoint.com/html-login-form
![[Pasted image 20240305161915.png]]
esta es la pagina que tendremos en el lado de producción por lo que intentaremos hacer un ssrf.
Para seguir el ejemplo podemos hacer una copia de esta plantilla dentro del contendor en la carpeta /tmp, haciendo algunos cambios dentro de estas en las cuales pueden salir algunas credenciales a testear, entre otras cosas: 
![[Pasted image 20240305164440.png]]
esto esta publico: 
![[Pasted image 20240305164550.png]]
ahora si le agregamos al final un --bind 127.0.0.1, quedando de la siguiente forma el comando: 
```
python3 -m http.server 4646 --bind 127.0.0.1
```
podemos ver que ya no podemos ver la pagina correctamente: 
![[Pasted image 20240305165838.png]]
si hacemos las pruebas correspondientes en nmap veremos: 
![[Pasted image 20240305165643.png]]
si hacemos una pruba dentro del puerto 80 a la ip 127.17.0.2, podemos ver que esta abierto.
pero si lo hacemos al puerto 4646, podremos ver que no lo está:
![[Pasted image 20240305165806.png]]
pero si vamos a la carpeta raíz del contendor y observamos la utilidad que esta en php que nos permite poner un dominio dentro de la url.
![[Pasted image 20240305170345.png]]
estamos listando el contenido del localhost, vemos lo mismo que en la carpeta raíz.
ahora podemos hacer un fuzzing de la pagina para ver que nos representa: 

``wfuzz -c -t 200 -z range,1-65535 "http://172.17.0.2/utility.php?url=http://127.0.0.1:FUZZ"``

![[Pasted image 20240305173844.png]]
vemos que todas las paginas nos mandan la misma cantidad de caracteres por lo que quizás sea bueno quitar estas respuestas utilizando --hl=3:
```
wfuzz -c --hl=3 -t 200 -z range,1-65535 "http://172.17.0.2/utility.php?url=http://127.0.0.1:FUZZ"
```
ahora podemos ver correctamente el input: 
![[Pasted image 20240305174107.png]]
y  vemos que estan los dos puertos abiertos el 80 y el 4646. 
![[Pasted image 20240305174149.png]]
si en la misma url le colocamos el /login.html veremos lo siguiente: 
![[Pasted image 20240305174319.png]]
Este el el primer ejemplo.






---------------
## Datos de la clase: 

El **Server-Side Request Forgery** (**SSRF**) es una vulnerabilidad de seguridad en la que un atacante puede forzar a un servidor web para que realice solicitudes HTTP en su nombre.

En un ataque de SSRF, el atacante utiliza una entrada del usuario, como una URL o un campo de formulario, para enviar una solicitud HTTP a un servidor web. El atacante manipula la solicitud para que se dirija a un servidor vulnerable o a una red interna a la que el servidor web tiene acceso.

El ataque de SSRF puede permitir al atacante acceder a información confidencial, como contraseñas, claves de API y otros datos sensibles, y también puede llegar a permitir al atacante (en función del escenario) ejecutar comandos en el servidor web o en otros servidores en la red interna.

Una de las **diferencias** clave entre el **SSRF** y el **CSRF** es que el SSRF se ejecuta en el servidor web en lugar del navegador del usuario. El atacante **no necesita engañar a un usuario legítimo** para hacer clic en un enlace malicioso, ya que puede enviar la solicitud HTTP directamente al servidor web desde una fuente externa.

Para prevenir los ataques de SSRF, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario y limiten el acceso del servidor web a los recursos de la red interna. Además, los servidores web deben ser configurados para limitar el acceso a los recursos sensibles y restringir el acceso de los usuarios no autorizados.

En esta clase, estaremos utilizando **Docker** para crear **redes personalizadas** en las que podremos simular un escenario de **red interna**. En esta red interna, intentaremos a través del SSRF apuntar a un recurso existente que no es accesible externamente, lo que nos permitirá explorar y comprender mejor la explotación de esta vulnerabilidad.

Para crear una nueva red en Docker, podemos utilizar el siguiente comando:

➜ `docker network create --subnet=<subnet> <nombre_de_red>`

Donde:

- **subnet**: es la dirección IP de la subred de la red que estamos creando. Es importante tener en cuenta que esta dirección IP debe ser única y no debe entrar en conflicto con otras redes o subredes existentes en nuestro sistema.
- **nombre_de_red**: es el nombre que le damos a la red que estamos creando.

Además de los campos mencionados anteriormente, también podemos utilizar la opción ‘**–driver**‘ en el comando ‘docker network create’ para especificar el controlador de red que deseamos utilizar.

Por ejemplo, si queremos crear una red de tipo “**bridge**“, podemos utilizar el siguiente comando:

➜ `docker network create --subnet=<subnet> --driver=bridge <nombre_de_red>`

En este caso, estamos utilizando la opción ‘**–driver=bridge**‘ para indicar que deseamos crear una red de tipo “**bridge**“. La opción –driver nos permite especificar el controlador de red que deseamos utilizar, que puede ser “**bridge**“, “**overlay**“, “**macvlan**“, “**ipvlan**” u otro controlador compatible con Docker.
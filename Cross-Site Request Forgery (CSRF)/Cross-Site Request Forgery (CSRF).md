## Mis apuntes: 

¿Que es un CSRF?
El CSRF es un tipo de exploit malicioso de un sitio web en el que comandos no autorizados son transmitidos por un usuario en el cual el sitio web confía.​ Esta vulnerabilidad es conocida también por otros nombres como XSRF, enlace hostil, ataque de un clic, secuestro de sesión, y ataque automático.

comenzamos instalando el laboratorio con docker-compose, para luego tenemos que agregar lo siguiente en el /etc/hosts:
![[Pasted image 20240301154244.png]]
ahora que tenemos esto en el /etc/hosts podemos ver: 
![[Pasted image 20240301165354.png]]
bueno para hacer este laboratorio nos dan 2 credenciales: 
![[Pasted image 20240301175704.png]]
nos conectaremos como alice primero: 
![[Pasted image 20240301175807.png]]
una vez dentro vemos que es como un estilo de facebook pero más malo, y si vamos revisando podremos ver distintas cosas: 
Perfil:
![[Pasted image 20240301175847.png]]
miembros: 
![[Pasted image 20240301175907.png]]
ahí podemos intentar agregar a los otros usuarios:
![[Pasted image 20240301175952.png]]
pero si hacemos hovering es decir poner el mouse en el boton de añadir amigos en la esquina inferior izquierda veremos informacion en la cual se vera el identificador del usuario: 
![[Pasted image 20240301180131.png]]
ahí podemos ver que Boby tiene el identificador 57.
podemos ver el nuestro al meternos en nuestro perfil haciendo ctrl + u, veremos que hay una seccion donde dice guid: 
![[Pasted image 20240301180347.png]]
ahora si editamos el perfil y queremos ver el post que se envía al servidor lo haremos por burpsuite: 
![[Pasted image 20240303005428.png]]
Dentro de la información que vemos en pantalla podemos ver que esta el elgg_token y el elgg_ts que son identificadores, que si no los supieramos no sabemos si funcionaria la peticion, por lo que al cambiarlo a GET podemos intentar hacer la peticion sin estos parametros para ver si funcionan  o no.
ahora para intentar hacer un csrf intentaremos hacer esto pero a través de una petición GET: 
![[Pasted image 20240303010442.png]]
la respuesta del servidor es: 
![[Pasted image 20240303010628.png]]
si le damos al boton follow redirection veremos :
![[Pasted image 20240303010715.png]]
que la petición se completo normalmente.
![[Pasted image 20240303010808.png]]
ahora podemos ver que en el nombre de usuario sale el que enviamos por la peticion get.
Este todavía no es un CSRF como tal, por que estamos haciendo solo con nuestro propio contenido, en el caso de que queramos hacerlo a un tercero podemos intentarlo y pasara lo siguiente: 
en este caso intentaremos cambiar el nombre de samy, que tiene guid=59
![[Pasted image 20240303012944.png]]
podemos ver lo mismo que en el caso anterior solo que si vamos a la pagina real podremos ver que: 
![[Pasted image 20240303013648.png]]
Samy sigue con el mismo nombre, pero ahora como Samy, recordando que las credenciales son samy:seedsamy, le enviaremos un mensaje con la url de la peticion por GET  del cambio de nombre de Alice, pero primero tenemos que ver si el editor nos permite enviar codigo html: 
![[Pasted image 20240304114336.png]]
con esto podemos asegurarnos de que el mensaje nos lo mostrará con la representación de las etiquetas.
ahora podemos hacer una imagen que haga algo por detrás sin necesariamente mostrar una imagen de la siguiente manera: 
![[Pasted image 20240304115427.png]]
si nos metemos a alice y vemos el mensaje sale lo siguiente: 
![[Pasted image 20240304115535.png]]
cambia el nombre al ver la "imagen":
![[Pasted image 20240304115737.png]]
ahora si hacemos lo mismo a través de la url podemos ver que si funciona: 
![[Pasted image 20240304121238.png]]
esa es parte de la url: 
```
www.seed-server.com/action/profile/edit?name=Se%20tenso&description=&accesslevel%5bdescription%5d=2&briefdescription=&accesslevel%5bbriefdescription%5d=2&location=&accesslevel%5blocation%5d=2&interests=&accesslevel%5binterests%5d=2&skills=&accesslevel%5bskills%5d=2&contactemail=&accesslevel%5bcontactemail%5d=2&phone=&accesslevel%5bphone%5d=2&mobile=&accesslevel%5bmobile%5d=2&website=&accesslevel%5bwebsite%5d=2&twitter=&accesslevel%5btwitter%5d=2&guid=56
```
![[Pasted image 20240304121323.png]]
Sabiendo esto quizás también podamos cambiar la contraseña haciendo este tipo de peticiones.
![[Pasted image 20240304122600.png]]
podemos intentar capturar la petición por post que se hace y ver si al convertirla por get podemos enviarla y cambiar la contraseña: 
![[Pasted image 20240304123639.png]]
si vemos eso podemos ver que no funciona por que es incorrecta la current password, pero si eliminamos el current password podemos ver si pasa algo: 
![[Pasted image 20240304123740.png]]
vemos que no se puede por lo que esta bien programada esta parte del programa.
ahora esto puede emplearse con muchos parametros dentro de una web, por ejemplo podemos hacer que por un mensaje puedan agregar a una persona de amigo: 
![[Pasted image 20240304125022.png]]
con esto podemos ver que no son necesario todos los codigos "unicos" llamados tokens, para poder agregar a un amigo por lo que podemos hacer lo mismo que con el cambio de nombre: 
![[Pasted image 20240304131944.png]]
vemos la publicación, si hacemos la inspección de este mensaje podemos ver lo siguiente: 
![[Pasted image 20240304132025.png]]
esta es la linea de la imagen, que por lo que vemos, hace una acción de agregar a un amigo con el id = 57. 
si vemos los amigos que tenemos, podemos ver que se agrego uno: 
![[Pasted image 20240304132158.png]]
que es boby, si vemos el id es igual al que se mando en el mensaje.
Esto se puede testear en muchas partes dentro de la aplicación.

-------------
## Datos de la clase: 

--------------------------------------------------

**AVISO (Actualización 11/05/2023)**: Si a la hora de hacer el ‘**docker-compose up -d**‘, os salta un error de tipo: “**networks.net-10.9.0.0 value Additional properties are not allowed (‘name’ was unexpected)**“, lo que tenéis que hacer es en el archivo ‘**docker-compose.yml**‘, borrar la línea número 41, la que pone “**name: net-10.9.0.0**“.

Con hacer esto, ya podréis desplegar el laboratorio sin ningún problema.

------------------------------------------------

El **Cross-Site Request Forgery** (**CSRF**) es una vulnerabilidad de seguridad en la que un atacante **engaña** a un usuario legítimo para que realice una acción no deseada en un sitio web sin su conocimiento o consentimiento.

En un ataque CSRF, el atacante engaña a la víctima para que haga clic en un enlace malicioso o visite una página web maliciosa. Esta página maliciosa puede contener una solicitud HTTP que realiza una acción no deseada en el sitio web de la víctima.

Por ejemplo, imagina que un usuario ha iniciado sesión en su cuenta bancaria en línea y luego visita una página web maliciosa. La página maliciosa contiene un formulario que envía una solicitud HTTP al sitio web del banco para transferir fondos de la cuenta bancaria del usuario a la cuenta del atacante. Si el usuario hace clic en el botón de envío sin saber que está realizando una transferencia, el ataque CSRF habrá sido exitoso.

El ataque CSRF puede ser utilizado para realizar una amplia variedad de acciones no deseadas, incluyendo la transferencia de fondos, la modificación de la información de la cuenta, la eliminación de datos y mucho más.

Para prevenir los ataques CSRF, los desarrolladores de aplicaciones web deben implementar medidas de seguridad adecuadas, como la inclusión de tokens CSRF en los formularios y solicitudes HTTP. Estos tokens CSRF permiten que la aplicación web verifique que la solicitud proviene de un usuario legítimo y no de un atacante malintencionado (aunque cuidadín que también se pueden hacer cositas con esto).

Os compartimos a continuación el enlace al comprimido ZIP que utilizamos en esta clase para desplegar el laboratorio donde practicamos esta vulnerabilidad:

- **Lab Setup**: [https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip)
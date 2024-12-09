## Datos de la clase: 
El ataque de asignación masiva (**Mass Assignment Attack**) se basa en la manipulación de parámetros de entrada de una solicitud HTTP para crear o modificar campos en un objeto de modelo de datos en la aplicación web. En lugar de agregar nuevos parámetros, los atacantes intentan explotar la funcionalidad de los parámetros existentes para modificar campos que no deberían ser accesibles para el usuario.

Por ejemplo, en una aplicación web de gestión de usuarios, un formulario de registro puede tener campos para el nombre de usuario, correo electrónico y contraseña. Sin embargo, si la aplicación utiliza una biblioteca o marco que permite la asignación masiva de parámetros, el atacante podría manipular la solicitud HTTP para agregar un parámetro adicional, como el nivel de privilegio del usuario. De esta manera, el atacante podría registrarse como un usuario con privilegios elevados, simplemente agregando un parámetro adicional a la solicitud HTTP.

A continuación, se os proporciona el enlace directo al proyecto **Juice Shop** en Docker Hub, el cual nos permitirá desplegar un laboratorio vulnerable donde poder practicar esta vulnerabilidad:

- **Juice Shop**: [https://hub.docker.com/r/bkimminich/juice-shop](https://hub.docker.com/r/bkimminich/juice-shop)


----------------
## Mis apuntes: 

Para montarnos este laboratorio tenemos que entrar en el link que nos entrega el profesor en la clase de docker juice shop, copiamos el comando que aparece ahí `docker pull bkimminich/juice-shop` 
este nos cargará una imagen de la cual nosotros haremos un docker run de la siguiente manera: `docker run -dit --name JuiceShop bkimminich/juice-shop
`
por lo que llegaremos a lo que aparece en la imagen:
![[Pasted image 20240521215901.png]]
para finalizar el montado del laboratorio tenemos que hacer un docker run de la siguiente manera: 
`docker run -dit -p 3000:3000 --name JuiceShop bkimminich/juice-shop`
Una vez montado el laboratorio, podemos encontrarnos con lo siguiente: 
![[Pasted image 20240522124835.png]]
ahora podemos ver que en esta pagina tenemos acceso a un login, y dentro de ese tenemos acceso a una pagina para registrarnos.
![[Pasted image 20240522132244.png]]
Ahora podemos intentar interceptar la petición con burpsuite, para poder ver como se maneja la petición. 
dentro de la herramienta tendremos lo siguiente: 
![[Pasted image 20240522132632.png]]
podemos ver que se manda como datos json.
Al mandar la petición podemos ver la respuesta que nos entrega el servidor en este caso podemos ver la siguiente información: 
![[Pasted image 20240522135633.png]]
podemos ver que el estatus es success, es decir que se registro el usuario correctamente, dentro de la data, podemos ver que tiene un rol llamado customer, algo que no nos pregunta en la ficha de inscripción, para hacer el ataque de asignación masiva tenemos que forzar poner un parametro que no esta permitido o que no esta dentro de la encuesta de la siguiente manera: 
![[Pasted image 20240522140358.png]]
si dentro de la petición mandamos un parametro role que contiene como dato admin, podemos ver que el servidor como respuesta nos mandara un cambio dentro de la cuenta, por que nos dirá que estamos como admin dentro del sistema. 
este es el primer ejemplo, el segundo que podemos hacer es con el siguiente contenedor. 
`docker pull blabla1337/owasp-skf-lab:parameter-binding`
![[Pasted image 20240522143706.png]]
después de eso tenemos que hacer un Docker run con el siguiente comando: `docker run -dit -p 3000:3000 blabla1337/owasp-skf-lab:parameter-binding`
cuando ya aplicamos esto dentro de los comandos podemos entrar a la pagina: 172.17.0.2:5000 para ver que hay dentro. 
![[Pasted image 20240522151810.png]]
encontramos esta pagina, donde hay 2 usuarios uno que es un usuario normal y otro que es un usuario administrador.
podemos entrar al panel de estos usuarios para poder editar la información, pero en este caso también interceptaremos esta peticion al servidor para ver que nos muestra. 
![[Pasted image 20240522152104.png]]
vemos que esta es la petición si hacemos un "ctrl + r" podemos mandar la peticion al repeater, donde podemos trabajar con fuerza bruta.
![[Pasted image 20240522152648.png]]
en este caso pudimos ver que cual es la estructura de esta petición por lo que copiamos y pegamos parte de esta estructura y para saber si somos adminsitradores o ganar esos privilegios quisimos hacer que el paremetro ``user[is_admin] = true`` con esto le decimos al servidor que queremos cambiar el parametro is_admin del usuario que tiene dicho token. 
![[Pasted image 20240522152908.png]]
y vemos que en la pagina cambia tambien y guest es administrador también. 
dentro del codigo esto esta de esta forma:
![[Pasted image 20240522153000.png]]
donde podemos ver que tiene un metodo actualizar, donde requiere datos del usuario, pero permite todo tipo de parametros, por lo que no esta sanitizado.
para sanitizarlo se puede utilizar una restriccion en el cual diremos que parametros queremos que se cambien, en este caso queremos que se actualize el username y el titilo.
ejemplo de sanitización: 
`params.require(:user).permit(:usename,:title)`.


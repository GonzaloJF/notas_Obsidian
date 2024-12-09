## Datos de la clase: 

El ataque **Prototype Pollution** es una técnica de ataque que aprovecha las vulnerabilidades en la implementación de objetos en JavaScript. Esta técnica de ataque se utiliza para modificar la propiedad “**prototype**” de un objeto en una aplicación web, lo que puede permitir al atacante ejecutar código malicioso o manipular los datos de la aplicación.

En JavaScript, la propiedad “prototype” se utiliza para definir las propiedades y métodos de un objeto. Los atacantes pueden explotar esta característica de JavaScript para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.

El ataque de Prototype Pollution se produce cuando un atacante modifica la propiedad “prototype” de un objeto en una aplicación web. Esto se puede lograr mediante la manipulación de datos que se envían a través de formularios o solicitudes AJAX, o mediante la inserción de código malicioso en el código JavaScript de la aplicación.

Una vez que el objeto ha sido manipulado, el atacante puede ejecutar código malicioso en la aplicación, manipular los datos de la aplicación o tomar el control de la sesión de un usuario. Por ejemplo, un atacante podría modificar la propiedad “prototype” de un objeto de autenticación de usuario para permitir el acceso a una cuenta sin la necesidad de una contraseña.

El impacto de la explotación del ataque Prototype Pollution puede ser significativo, ya que los atacantes pueden tomar el control de la aplicación o comprometer los datos de los usuarios. Además, dado que el ataque se basa en vulnerabilidades en la implementación de objetos en JavaScript, puede ser difícil de detectar y corregir.

A continuación, se proporciona el enlace directo al proyecto de Github que utilizamos en esta clase para desplegar un entorno vulnerable con el que poder practicar:

- **SKF-LABS**: [https://github.com/blabla1337/skf-labs](https://github.com/blabla1337/skf-labs)


---
## Mis Apuntes: 
Para el siguiente ejemplo nos montaremos un laboratorio, cuya pantalla principal es la siguiente: 
![[Pasted image 20240320161307.png]]
lo primero que haremos es crear un usuario y nos logearemos con dicha cuenta: 
![[Pasted image 20240320162817.png]]
tenemos la siguiente respuesta al loguearnos: 
![[Pasted image 20240320162846.png]]
vemos nuestro nombre pero vemos que Admin esta vacío, pero también vemos que hay un apartado de envió de mensajes al admin.
![[Pasted image 20240320163015.png]]
bueno ahora tenemos que analizar un poco el codigo: 
![[Pasted image 20240320163757.png]]
podemos ver que hay 2 tipos de usuarios en el cual tienen un tipo de dato booleano que se nos muestra en true o en false, que en este caso es admin,  por lo que al registrar podemos ver que solo nos consultan por el username y por la contraseña pero dejan libre este tipo de dato, es decir que no lo definen como un true o false. 
ahora podemos hacer una demostracion: 
![[Pasted image 20240320164841.png]]
podemos ver que tenemos una funcion merge en donde nos hace la union de dos datos para formar un solo dato, es decir que le estamos pasando la ip que tenemos y el body que creamos, en este caso podemos hacer el prototype pollution ya que dentro del cuerpo le agregamos una variable llamada `"__proto__"` que esto indica que le estamos pasando un parametro al prototipo del cuerpo y pueden heredar este tipo de datos los usuarios que no tengan definida esta variable en ese caso el  `"isAdmin"`.
![[Pasted image 20240320165902.png]]
este seria el ejemplo que se puede hacer, en el mensaje lo interceptamos con burpsuite, dentro de este mensaje le mandamos un prototipo con el tipo de variable admin y lo seteamos en true. 

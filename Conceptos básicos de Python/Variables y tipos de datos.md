## Datos de la clase: 

Las variables en Python son como nombres que se le asignan a los datos que manejamos. Piensa en una variable como un nombre que pones a un valor, para poder referirte a él y utilizarlo en diferentes partes de tu código.

En la clase actual, vamos a enfocarnos en comprender las variables y algunos de los tipos de datos fundamentales en Python. Estos conceptos son esenciales, ya que nos permiten almacenar y manipular la información en nuestros programas.

**Variables**

Una variable en Python es como un nombre que se le asigna a un dato. No es necesario declarar el tipo de dato, ya que Python es inteligente para inferirlo.

**Cadenas (Strings)**

Las cadenas son secuencias de caracteres que se utilizan para manejar texto. Son inmutables, lo que significa que una vez creadas, no puedes cambiar sus **caracteres individuales**.

**Números**

Python maneja varios tipos numéricos, pero nos centraremos principalmente en:

- **Enteros (Integers)**: Números sin parte decimal.
- **Flotantes (Floats)**: Números que incluyen decimales.

**Listas**

Las listas son colecciones ordenadas y mutables que pueden contener elementos de diferentes tipos. Son ideales para almacenar y acceder a secuencias de datos.

Y para trabajar con estas listas, así como con cadenas y rangos de números, utilizaremos los bucles ‘**for**‘, que nos permiten iterar sobre cada elemento de una secuencia de manera eficiente.

Estas son solo algunas de las estructuras de datos con las que trabajaremos por el momento. A medida que avancemos en las próximas clases, exploraremos más tipos de datos y estructuras más complejas, ampliando nuestras herramientas para resolver problemas y construir programas más sofisticados.

---
## Mis apuntes: 

### Tipos de variables:
para ver los tipos de datos haremos distintos tipos de ejemplos para poder ver  como se declaran y como se ocupan estos tipos de datos: 
![[Pasted image 20240312003645.png]]
tenemos una variable cadena que es una cadena de string, que después se imprime la cadena de string.
![[Pasted image 20240312004539.png]]
con esto podemos ver que los números no se están comportando como números sino que como una cadena de caracteres. 
imprimimos en pantalla la cadena de ip y además podemos imprimir el tipo de datos que es, por lo que podemos comprobar que es de tipo string. 
En el siguiente ejemplo veremos lo que pasa si a una variable se le pone solo un numero, sin comillas y sin puntos.
![[Pasted image 20240312103449.png]]
podemos ver que al no colocarle comillas ni puntos se muestra el número en pantalla y también podemos ver que el tipo de variable es int, de integer, o número entero.
entonces ahora si modificamos la variable y en ves de poner un numero entero, colocamos un numero decimal podemos ver lo siguiente: 
![[Pasted image 20240312103808.png]]
podemos ver que cambio el tipo de variable a flotante, ya que es un numero decimal. 
hay un concepto llamado Type Casting, que sirve para identificar que tipo de dato queremos que sea la variable, en este ejemplo cambiaremos la variable representando un numero entero como float. 
![[Pasted image 20240312104239.png]]
y si le agregamos el parámetro float() al numero veremos lo siguiente: 
![[Pasted image 20240312104321.png]]
esto también funciona con los otros tipos de variables, ejemplo str() para que se convierta en una cadena, el parámetro int() para convertir un float en entero y quitar la parte decimal. 

### Listas: 
las listas son un tipo de dato, que puede contener, desde nada a muchos datos dentro de estos esta representado de la siguiente forma: 
![[Pasted image 20240312110114.png]]
podemos ver lo siguiente en este ejemplo, la lista se puede inicializar con un par de corchetes "[ ]", para agregar datos a una lista se puede utilizar variable.append(), y para imprimir en pantalla los datos de acuerdo a una cierta posición tenemos que poner `print(variable[indice])`.
para que sea más cómodo el visualizar los datos dentro de una lista se puede utilizar un bucle for para recorrer la lista y mostrar los datos de la siguiente forma: 
![[Pasted image 20240312110627.png]]
y lo bueno que tiene esto es que es totalmente escalable, si al agregar nuevos datos los mostrara también en pantalla. 
ahora si queremos hacerlo junto con un mensaje podemos hacer lo siguiente: 
![[Pasted image 20240312111054.png]]
podemos poner una "f" antes del mensaje que queremos poner y entre llaves la variable para que sepa que queremos mostrar la variable. 
![[Pasted image 20240312111339.png]]
con esto hasta podemos utilizar algunos comandos como "len" para poder mostrar el largo de la lista. 
si ya sabes la cantidad de elementos que se van a utilizar dentro de la lista, la lista se puede declarar ya con estos dentro de la lista, de la siguiente forma: 
![[Pasted image 20240312111613.png]]
otra forma que se puede utilizar para agregar más elementos a la lista es utilizando el metodo extend([]) de la siguiente manera: 
![[Pasted image 20240312111955.png]]
también podemos ver que hay otra forma que es ocupando el += como en el siguiente ejemplo: 
![[Pasted image 20240312112600.png]]
agregamos el 86, 87 dentro de los puertos abiertos. 
hay funciones internas de las listas para poder ordenarlas de menor a mayor de la siguiente forma: 
![[Pasted image 20240312112841.png]]
con la función sorted() podemos ver que la lista se ordena de menor a mayor.
también se pueden eliminar datos dentro de una lista: 
![[Pasted image 20240312113846.png]]
con la función `del variable[indice del dato a eliminar] `
![[Pasted image 20240312114204.png]]
con el parámetro `print(my_ports[:4])` al indicarle con el :4 le diremos al programa que nos muestre los primeros 4 elementos.
con el parámetro insert podemos insetar un valor a la lista en la posición que le indiquemos: 
podemos ver que se inserto correctamente: 
![[Pasted image 20240312114632.png]]
para saber el indice de un elemento podemos ocupar la función ``variable.index[elemento]``.
![[Pasted image 20240312115313.png]]





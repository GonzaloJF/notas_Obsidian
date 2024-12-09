## Datos de la clase: 

En esta clase nos sumergimos en dos conceptos fundamentales de la programación en Python que potencian la modularidad y la gestión eficaz de los datos dentro de nuestros programas.

**Funciones**

Las funciones son bloques de código reutilizables diseñados para realizar una tarea específica. En Python, se definen usando la palabra clave ‘**def**‘ seguida de un nombre descriptivo, paréntesis que pueden contener parámetros y dos puntos. Los parámetros son “**variables de entrada**” que pueden cambiar cada vez que se llama a la función. Esto permite a las funciones operar con diferentes datos y producir resultados correspondientes.

Las funciones pueden devolver valores al programa principal o a otras funciones mediante la palabra clave ‘**return**‘. Esto las hace increíblemente versátiles, ya que pueden procesar datos y luego pasar esos datos modificados a otras partes del programa.

**Ámbito de las Variables (Scope)**

El ámbito de una variable se refiere a la región de un programa donde esa variable es accesible. En Python, hay dos tipos principales de ámbitos:

- **Local**: Las variables definidas dentro de una función tienen un ámbito local, lo que significa que solo pueden ser accesadas y modificadas dentro de la función donde fueron creadas.
- **Global**: Las variables definidas fuera de todas las funciones tienen un ámbito global, lo que significa que pueden ser accesadas desde cualquier parte del programa. Sin embargo, para modificar una variable global dentro de una función, se debe declarar como global.

Durante esta clase, exploraremos cómo definir y llamar funciones, cómo pasar información a las funciones a través de argumentos, y cómo las variables interactúan con diferentes ámbitos. También veremos las mejores prácticas para definir funciones claras y concisas y cómo el correcto manejo del ámbito de las variables puede evitar errores y complicaciones en el código.

Comprender estos conceptos es esencial para escribir programas claros, eficientes y mantenibles en Python. Al finalizar la clase, tendrás una sólida comprensión de cómo estructurar tu código y cómo gestionar las variables para que tus programas funcionen de manera impecable.

---
## Mis apuntes: 

vamos a ir haciendo distintos ejemplos de funciones para saber como trabajan estas: 
![[Pasted image 20240318174812.png]]
esta es una de las formas de poder definir una funcion con una acción y como se llama: 
Ahora si queremos pasarle un parámetro a la funcion podemos llamarla de la siguiente forma: 
![[Pasted image 20240318174955.png]]
ahora si queremos realizar una operatoria podemos hacer lo siguiente: 
![[Pasted image 20240318175317.png]]
Ahora empezando con el ámbito de las variables tenemos 2 tipos de variables que se ocupan para las funciones, hay algunas variables locales y otras que son globales, podemos entender esto con el siguiente ejemplo:
![[Pasted image 20240318175925.png]]
con este ejemplo podemos ver que al llamar la función podemos ver ambas variables pero al momento de estar fuera de la función si llamamos a la variable local el programa no va a saber a quien llamar ya que esta variable.
También podemos modificar las variables globales dentro de una función de la siguiente forma: 
![[Pasted image 20240318181405.png]]

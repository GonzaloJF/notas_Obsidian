## Datos de la clase: 
Dentro del paradigma de la Programación Orientada a Objetos en Python, existen conceptos avanzados como los decoradores, métodos de clase y métodos estáticos que enriquecen y expanden las posibilidades de cómo interactuamos con las clases y sus instancias.

**Decoradores**

Los decoradores son una herramienta poderosa en Python que permite modificar el comportamiento de una función o método. Funcionan como “envoltorios”, que agregan funcionalidad antes y después del método o función decorada, sin cambiar su código fuente. En POO, los decoradores son frecuentemente utilizados para agregar funcionalidades de manera dinámica a los métodos, como la sincronización de hilos, la memorización de resultados o la verificación de permisos.

**Métodos de Clase**

Un método de clase es un método que está ligado a la clase y no a una instancia de la clase. Esto significa que el método puede ser llamado sobre la clase misma, en lugar de sobre un objeto de la clase. Se definen utilizando el decorador ‘**@classmethod**‘ y su primer argumento es siempre una referencia a la clase, convencionalmente llamada ‘**cls**‘. Los métodos de clase son utilizados a menudo para definir métodos “factory” que pueden crear instancias de la clase de diferentes maneras.

**Métodos Estáticos**

Los métodos estáticos, definidos con el decorador ‘**@staticmethod**‘, no reciben una referencia implícita ni a la instancia (self) ni a la clase (cls). Son básicamente como funciones regulares, pero pertenecen al espacio de nombres de la clase. Son útiles cuando queremos realizar alguna funcionalidad que está relacionada con la clase, pero que no requiere acceder a la instancia o a los atributos de la clase.

Estos elementos de la POO en Python nos permiten crear abstracciones más claras y mantener el código organizado, modular y flexible, facilitando el mantenimiento y la extensibilidad del software.

----
## Mis apuntes: 
Comenzaremos con unos ejemplos didácticos para ver que se pueden hacer. 
![[Pasted image 20240520132636.png]]
en este caso en la clase rectángulo podemos ver que se juega con el decorador @property este hacer que el print lo tome como lo que es una propiedad dentro de la clase por lo que no es necesario llamarla como una función dentro de la clase, en el print podemos ver que se llama rect1.area, y no como rect1.area().
y veremos como respuesta lo siguiente: 
![[Pasted image 20240520140218.png]]
ahora también tenemos una decoración que se llama `_ _ str _ _ ` esta sirve para poder instanciar al objeto y poder colocar como funcion lo que queremos ver si instanciamos el objeto.
por ejemplo: 
![[Pasted image 20240520140647.png]]
podemos ver que en la función antes mencionada tenemos las propiedades del rectángulo ya que nos da el ancho y el alto del mismo, por lo que si le hacemos un print a rect1 que es el objeto de tipo rectángulo deberíamos ver las propiedades de dicho triangulo.
 ![[Pasted image 20240520140911.png]]
podemos ver claramente que se nos dan las propiedades de dicha función. 
También tenemos el método `__eq__` que es una forma de poder hacer comparativas matemáticas, es decir si queremos ver que el ancho y el alto de 2 rectángulos son iguales podemos realizar esa pregunta en este método: 
![[Pasted image 20240520144254.png]]
En este ejemplo podemos ver que se hace la verificación del ancho y del alto, y veremos que en la respuesta nos regresa true o false. 
![[Pasted image 20240520144323.png]]
podemos ver que nos sale un false en el caso de que no sean iguales.
Otro ejemplo que haremos es de una libreria: 

![[Pasted image 20240520165120.png]]
En este ejemplo veremos como se hace un metodo estatico para una clase, esto corresponde a que podremos tener metodos que no se utilizan mediante el objeto sino que mediante la llamada de la propia clase, por lo  que es importante tener en cuenta el decorador que esta arriba de nuestro metodo es_bestseller en este caso. 
si lo ejecutamos podemos ver que nos entrega un true o un false. 
![[Pasted image 20240520165531.png]]
en este caso es un false. 
Este es un caso para ver que tambien hay un classmethod, esto es que depende de la clase pueden variar algunas variables a tener en cuenta dentro de nuestros metodos, como podemos ver que hay una clase libro y una clase libro digital, podemos ver que libro digital hereda los atributos de la clase libro pero cambia la cantidad de iva de estos por lo que si hacemos funcionar esto nos enviara una respuesta del tipo: 
![[Pasted image 20240520183219.png]]
esta es la respuesta que tendremos: 
![[Pasted image 20240520183425.png]]

## Datos de la clase: 
En esta clase, nos adentraremos en los conjuntos, conocidos en Python como ‘**sets**‘. Los conjuntos son una colección de elementos sin orden y sin elementos repetidos, inspirados en la teoría de conjuntos de las matemáticas. Son ideales para la gestión de colecciones de elementos únicos y operaciones que requieren eliminar duplicados o realizar comparaciones de conjuntos.

**Características de los Conjuntos**

- **Unicidad**: Los conjuntos automáticamente descartan elementos duplicados, lo que los hace perfectos para recolectar elementos únicos.
- **Desordenados**: A diferencia de las listas y las tuplas, los conjuntos no mantienen los elementos en ningún orden específico.
- **Mutabilidad**: Los elementos de un conjunto pueden ser agregados o eliminados, pero los elementos mismos deben ser inmutables (por ejemplo, no puedes tener un conjunto de listas, ya que las listas se pueden modificar).

**Operaciones con Conjuntos**

Exploraremos las operaciones básicas de conjuntos que Python facilita, como:

- **Adición y Eliminación**: Añadir elementos con ‘**add()**‘ y eliminar elementos con ‘**remove()**‘ o ‘**discard()**‘.
- **Operaciones de Conjuntos**: Realizar uniones, intersecciones, diferencias y diferencias simétricas utilizando métodos o operadores respectivos.
- **Pruebas de Pertenencia**: Comprobar rápidamente si un elemento es miembro de un conjunto.
- **Inmutabilidad Opcional**: Usar el tipo ‘**frozenset**‘ para crear conjuntos que no se pueden modificar después de su creación.

**Uso de Conjuntos en Python**

- **Eliminación de Duplicados**: Son útiles cuando necesitas asegurarte de que una colección no tenga elementos repetidos.
- **Relaciones entre Colecciones**: Facilitan la comprensión y el manejo de relaciones matemáticas entre colecciones, como subconjuntos y superconjuntos.
- **Rendimiento de Búsqueda**: Proporcionan una búsqueda de elementos más rápida que las listas o las tuplas, lo que es útil para grandes volúmenes de datos.

**Buenas Prácticas**

Discutiremos cuándo es beneficioso usar conjuntos en lugar de otras estructuras de datos y cómo su uso puede influir en la eficiencia del programa.

Al final de esta clase, tendrás una comprensión completa de los conjuntos en Python y cómo pueden ser utilizados para hacer tu código más eficiente y lógico, aprovechando sus propiedades únicas para manejar datos. Con este conocimiento, podrás implementar estructuras de datos complejas y operaciones que requieren lógica de conjuntos.

----
## Mis Apuntes: 

![[Pasted image 20240325175027.png]]
esta es la forma de inicializar un conjunto. 
Se pueden agregar datos al conjunto de la siguiente forma: 
![[Pasted image 20240326114322.png]]
Al agregar datos, no siempre python lo hará de manera ordenada, como por ejemplo cuando se agrega el numero 8  a este conjunto: 
![[Pasted image 20240326114512.png]]
otra forma de agregar datos al conjunto es con la funcion update: 
![[Pasted image 20240326114641.png]]
puedes eliminar elementos con la funcion remove: 
![[Pasted image 20240326115248.png]]
en este caso eliminamos el elemento que contenia el numero 3. 
hay una funcion en la cual, si hay un elemento que no queremos que entre a nuestro conjunto podemos ocupar la funcion discard(), se utiliza de la siguiente forma: 
![[Pasted image 20240326115535.png]]
tenemos una función de intersección de conjuntos en el cual se guarda en el conjunto final.
![[Pasted image 20240326123310.png]]
podemos hacer uniones entre un conjunto y otros, como por ejemplo: 
![[Pasted image 20240326132931.png]]
podemos ver que junta todos los elementos de ambos conjuntos sin repetir los elementos que estan ya en el primer conjunto de elementos. 
![[Pasted image 20240326133542.png]]
También se pueden hacer type cast para hacer un conjunto de una lista como por ejemplo: 
![[Pasted image 20240326150913.png]]
el hacer búsquedas dentro de un conjunto 
![[Pasted image 20240326155552.png]]

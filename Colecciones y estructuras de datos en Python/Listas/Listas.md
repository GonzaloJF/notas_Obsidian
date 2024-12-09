## Datos de la clase: 

En esta clase, nos sumergiremos en profundidad en uno de los tipos de datos más versátiles y utilizados en Python.

Las listas son estructuras de datos que nos permiten almacenar secuencias ordenadas de elementos. Son mutables, lo que significa que podemos modificarlas después de su creación, y son dinámicas, permitiéndonos añadir o quitar elementos de ellas.

**Características de las Listas**

Vamos a explorar las características clave de las listas en Python, que incluyen su capacidad para:

- Almacenar datos heterogéneos, es decir, pueden contener elementos de diferentes tipos (enteros, cadenas, flotantes y más) dentro de una misma lista.
- Ser indexadas y cortadas, lo que permite acceder a elementos específicos de la lista directamente a través de su índice.
- Ser anidadas, es decir, una lista puede contener otras listas como elementos, lo que permite crear estructuras de datos complejas como matrices.

**Operaciones con Listas**

También cubriremos las operaciones fundamentales que se pueden realizar con listas, como:

- Añadir elementos con métodos como ‘**append()**‘ y ‘**extend()**‘.
- Eliminar elementos con métodos como ‘**remove()**‘ y ‘**pop()**‘.
- Ordenar las listas con el método ‘**sort()**‘ o la función incorporada ‘**sorted()**‘.
- Invertir los elementos con el método ‘**reverse()**‘ o la sintaxis de corte ‘**[::-1]**‘.
- Comprender las comprensiones de listas, una forma “pythonica” de crear y manipular listas de manera concisa y eficiente.

**Métodos de Listas**

Profundizaremos en la rica gama de métodos que Python ofrece para trabajar con listas y cómo estos métodos pueden ser utilizados para manipular listas de acuerdo a nuestras necesidades.

**Buenas Prácticas**

Discutiremos las mejores prácticas en el manejo de listas, incluyendo cómo y cuándo usar listas en comparación con otros tipos de colecciones en Python, como tuplas, conjuntos y diccionarios.

Al final de esta clase, tendrás un conocimiento profundo de las listas en Python y estarás equipado con las técnicas para manejarlas eficazmente en tus programas. Con esta base sólida, podrás manipular colecciones de datos con confianza y aplicar esta habilidad central en tareas como la manipulación de datos, la automatización y el desarrollo de algoritmos.

---
## Mis apuntes: 

vamos a hacer un script para poder ir practicando lo de las listas: 

primero crearemos una lista con puertos abirtos: 
![[Pasted image 20240323150309.png]]
al imprimir esta lista nos saldra el array entero sin ordenar. 
ahora si le colocamos dentro del print la funcion len(), podemos ver la longitud de esta lista: 
![[Pasted image 20240323150451.png]]
podemos agregar datos ocupando la funcion append(), y podemos ver que la lista se alarga y tambien la funcion len(), la cuenta: 
![[Pasted image 20240323151539.png]]
se puede recorrer el array con un bucle for de los puertos: 
![[Pasted image 20240323153726.png]]
ahora también podemos remover uno de los elementos de la lista de la siguiente forma
![[Pasted image 20240323154455.png]]
también se pueden ordenar los datos de una lista de la con la funcion sort(): 
![[Pasted image 20240323160445.png]]
también se pueden invertir los elementos: 
![[Pasted image 20240323171240.png]]
se pueden acceder a los distintos elementos de la lista entregándole el indice, como por ejemplo:
![[Pasted image 20240323191203.png]]
en este caso se hace una sublista desde el inicio de la lista hasta el elemento con el indice 3. 
hay un caso en el que empieza la lista desde el que tiene indice 3 hasta el final de la lista.
 ![[Pasted image 20240325122904.png]]
 también se pueden englobar rangos: 
 ![[Pasted image 20240325132116.png]]
 forma de recorrer la lista con un bucle while: 
 ![[Pasted image 20240325132532.png]]
 para hacer una enumeración con el índice de cada elemento y el elemento de la lista podemos ocupar la función enumerate: 
 ![[Pasted image 20240325133338.png]]
 con la funcion `<variable>.upper ó <variable>.lower` podemos hacer que todos los elementos de la lista esten en mayusculas o en minusculas. 
 ![[Pasted image 20240325133952.png]]
 para poder mezclar 2 listas podemos ocupar la funcion zip(): 
 ![[Pasted image 20240325153657.png]]
 Otra forma de eliminar datos dentro de una lista es ocupando la funcion "del" de la siguiente forma: 
 ![[Pasted image 20240325161030.png]]
 ahora si queremos limpiar la lista completa podemos ocupar la función clear(): 
 ![[Pasted image 20240325161347.png]]
 con la funcion pop eliminamos el ultimo elemento de la lista: 
 ![[Pasted image 20240325163453.png]]
 también se puede poner que usuario eliminar y se puede guardar en una variable para saber que elemento se elimino. 
 ![[Pasted image 20240325164228.png]]
 ahora tambien podemos modificar el contenido de un usuario por ejemplo: 
 ![[Pasted image 20240325165139.png]]
si queremos insertar un elemento en una posición especifica podemos hacerlo con insert(). 
![[Pasted image 20240325165349.png]]
ahora si tenemos 2 listas y queremos unirlas podemos hacerlo con extend:
![[Pasted image 20240325165541.png]]

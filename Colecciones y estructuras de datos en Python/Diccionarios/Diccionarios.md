## Datos de la clase: 
En esta clase, nos centraremos en los diccionarios, una de las estructuras de datos más poderosas y flexibles de Python. Los diccionarios en Python son colecciones desordenadas de pares clave-valor. A diferencia de las secuencias, que se indexan mediante un rango numérico, los diccionarios se indexan con claves únicas, que pueden ser cualquier tipo inmutable, como cadenas o números.

**Características de los Diccionarios**

- **Desordenados**: Los elementos en un diccionario no están ordenados y no se accede a ellos mediante un índice numérico, sino a través de claves únicas.
- **Dinámicos**: Se pueden agregar, modificar y eliminar pares clave-valor.
- **Claves Únicas**: Cada clave en un diccionario es única, lo que previene duplicaciones y sobrescrituras accidentales.
- **Valores Accesibles**: Los valores no necesitan ser únicos y pueden ser de cualquier tipo de dato.

**Operaciones con Diccionarios**

Durante la clase, exploraremos cómo realizar operaciones básicas y avanzadas con diccionarios:

- **Agregar y Modificar**: Cómo agregar nuevos pares clave-valor y modificar valores existentes.
- **Eliminar**: Cómo eliminar pares clave-valor usando del o el método ‘**pop()**‘.
- **Métodos de Diccionario**: Utilizar métodos como ‘**keys()**‘, ‘**values()**‘, y ‘**items()**‘ para acceder a las claves, valores o ambos en forma de pares.
- **Comprensiones de Diccionarios**: Una forma elegante y concisa de construir diccionarios basados en secuencias o rangos.

**Uso de Diccionarios en Python**

- **Almacenamiento de Datos Estructurados**: Ideales para almacenar y organizar datos que están relacionados de manera lógica, como una base de datos en memoria.
- **Búsqueda Eficiente**: Los diccionarios son altamente optimizados para recuperar valores cuando se conoce la clave, proporcionando tiempos de búsqueda muy rápidos.
- **Flexibilidad**: Pueden ser anidados, lo que significa que los valores dentro de un diccionario pueden ser otros diccionarios, listas o cualquier otro tipo de dato.

**Buenas Prácticas**

Enfatizaremos las mejores prácticas para trabajar con diccionarios, incluyendo la selección de claves adecuadas y el manejo de errores comunes, como intentar acceder a claves que no existen.

Al final de esta clase, tendrás una comprensión completa de los diccionarios y estarás listo para utilizarlos para gestionar eficazmente los datos dentro de tus programas. Los diccionarios son una herramienta esencial en Python y saber cómo utilizarlos te abrirá la puerta a un nuevo nivel de programación.

---
## Mis apuntes: 

Lo primerio que haremos como ejemplo de este tipo de datos es saber como podemos declararlo y que tipo nos dice que es el interprete de python: 
![[Pasted image 20240515010747.png]]
para imprimir un solo dato del diccionario se puede hacer de la siguiente forma: 
![[Pasted image 20240515011421.png]].
para modificar un dato se puede hacer lo siguiente: 
![[Pasted image 20240515011648.png]]
para agregar un dato se hace de la misma forma: 
![[Pasted image 20240515011906.png]]
podemos eliminar datos de un diccionario de la siguiente forma: 
![[Pasted image 20240516011532.png]]
para comprobar claves dentro del diccionario se puede hacer con condicionales como los if: 
![[Pasted image 20240516012331.png]]
podemos ver que solo nos aparece la que es verdadera, la segunda no nos escribe la clave existe por que en realidad no existe. 
Se puede tambien hacer por keys, de la siguiente manera: 
![[Pasted image 20240516012720.png]]
de esta forma podemos enumerar que valores tiene cada una de las claves del diccionario. 
Podemos ver la cantidad de elementos que puede tener el diccionario con la siguiente función: 
![[Pasted image 20240516013137.png]]
también podemos limpiar los diccionarios de la siguiente forma:
![[Pasted image 20240516013347.png]]
De esta forma también podemos hacer operaciones dentro de los diccionarios: 
![[Pasted image 20240516120241.png]]
de esta forma podemos ver solo las claves y solo los valores: 
![[Pasted image 20240516120443.png]]
también se puede encontrar empleando la funcion get: 
![[Pasted image 20240516121916.png]]
podemos upgradear un diccionario ocupando la funcion update: 
![[Pasted image 20240516122647.png]]
podemos encontrar que se pueden hacer diccionarios dentro de otro diccionario.
![[Pasted image 20240516123217.png]]

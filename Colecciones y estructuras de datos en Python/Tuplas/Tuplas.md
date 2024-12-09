## Datos de la clase: 
En esta clase, dedicaremos nuestro enfoque a las tuplas, una estructura de datos fundamental en Python que comparte algunas similitudes con las listas, pero se distingue por su inmutabilidad.

Las tuplas son colecciones ordenadas de elementos que no pueden modificarse una vez creadas. Esta característica las hace ideales para asegurar que ciertos datos permanezcan constantes a lo largo del ciclo de vida de un programa.

**Características de las Tuplas**

- **Inmutabilidad**: Una vez que se crea una tupla, no puedes cambiar, añadir o eliminar elementos. Esta inmutabilidad garantiza la integridad de los datos que desea mantener constantes.
- **Indexación y Slicing**: Al igual que las listas, puedes acceder a los elementos de la tupla mediante índices y también puedes realizar operaciones de slicing para obtener subsecuencias de la tupla.
- **Heterogeneidad**: Las tuplas pueden contener elementos de diferentes tipos, incluyendo otras tuplas, lo que las hace muy versátiles.

**Operaciones con Tuplas**

Aunque no puedes modificar una tupla, hay varias operaciones que puedes realizar:

- **Empaquetado y Desempaquetado de Tuplas**: Las tuplas permiten asignar y desasignar sus elementos a múltiples variables de forma simultánea.
- **Concatenación y Repetición**: Similar a las listas, puedes combinar tuplas usando el operador ‘**+**‘ y repetir los elementos de una tupla un número determinado de veces con el operador ‘*****‘.
- **Métodos de Búsqueda**: Puedes usar métodos como ‘**index()**‘ para encontrar la posición de un elemento y ‘**count()**‘ para contar cuántas veces aparece un elemento en la tupla.

**Uso de Tuplas en Python**

- **Funciones y Asignaciones Múltiples**: Las tuplas son muy útiles cuando una función necesita devolver múltiples valores o cuando se realizan asignaciones múltiples en una sola línea.
- **Estructuras de Datos Fijas**: Se usan para crear estructuras de datos que no deben cambiar, como los días de la semana o las coordenadas de un punto en el espacio.

**Buenas Prácticas**

Abordaremos cuándo es más apropiado utilizar tuplas en lugar de listas y cómo la elección de una tupla sobre una lista puede afectar la claridad y la seguridad del código.

Al concluir esta clase, tendrás un entendimiento claro de qué son las tuplas, cómo y cuándo utilizarlas en tus programas, y las prácticas recomendadas para trabajar con este tipo de datos inmutable. Las tuplas son una herramienta poderosa en Python, y saber cómo utilizarlas te permitirá escribir código más seguro y eficiente.

---
## Mis Apuntes: 

Las tuplas son colecciones de datos que son inmutables, es decir que no se pueden modificar de una forma tan facil como las listas, por lo que tenemos algunas reglas para tener buenas practicas en ellas: 
![[Pasted image 20240325170345.png]]
esta es la forma correcta de declarar una tupla en este caso es con parentesis, y podemos ver que imprime todos los elementos de la tupla. 
para mostrar el primer elemento podemos hacerlo de la siguiente forma: 
![[Pasted image 20240325170658.png]]
se pueden englobar rangos de muestra: 
![[Pasted image 20240325170806.png]]
esto es exactamente igual que las listas. 
![[Pasted image 20240325171016.png]]
no podemos hacer modificación de los datos dentro de una tupla, no es igual que la listas. 
las tuplas soportan todo los tipos de datos dentro de ellas, como por ejemplo: 
![[Pasted image 20240325171238.png]]
podemos ver una cadena de caracteres, una lista, un booleano, entre los datos que están dentro de la tupla. 
También podemos ver que la tupla se pueden recorrer con un ciclo for, al igual que las listas. 
![[Pasted image 20240325171530.png]]
dentro de las funciones que se ocupan para insertar, modificar o eliminar elementos dentro de una lista, ninguno funciona para las tuplas ya que estas son inmutables. 
ahora si tenemos una tupla con algunos valores dentro de estas y se la queremos asignar a distintas variables independientes podemos hacer lo siguiente: 
![[Pasted image 20240325172253.png]]
lo que si se puede hacer es concatenar las tuplas, esto se hace de la siguiente manera: 
![[Pasted image 20240325172608.png]]
ahora tambien se puede hacer operaciones aritmeticas dentro de una tupla: 
![[Pasted image 20240325173117.png]]
la primera parte muestra 3 veces la misma tupla, y la segunda es una operacion para sacar solo los numeros pares: 


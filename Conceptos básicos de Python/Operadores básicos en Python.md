## Datos de la clase: 

Los operadores aritméticos son símbolos que Python utiliza para realizar cálculos matemáticos.

Los fundamentales son:

    Suma (+): No solo suma números, sino que también une secuencias como cadenas y listas, creando una nueva secuencia que es la combinación de ambas.
    Resta (-): Se utiliza para restar un número de otro. Con listas, su uso es menos directo y generalmente no se aplica como operador directo.
    Multiplicación (*): Cuando se multiplica un número por otro, obtenemos el producto. Con cadenas y listas, este operador repite los elementos la cantidad de veces especificada.
    División (/): Divide un número entre otro y el resultado es siempre un número flotante, incluso si los números son enteros.
    Exponente (**): Eleva un número a la potencia de otro. Por ejemplo, ‘2 ** 3‘ resultará en 8. Este operador es menos común en operaciones con cadenas o listas.

Operaciones con Cadenas

En Python, las cadenas son objetos que representan secuencias de caracteres y se pueden manipular usando operadores aritméticos:

    Concatenación (+): Une varias cadenas en una sola. Por ejemplo, ‘Hola’ + ‘ ‘ + ‘Mundo’ se convierte en ‘Hola Mundo’.
    Repetición (*): Crea repeticiones de la misma cadena. ‘Hola’ * 3 generará ‘HolaHolaHola’.

Operaciones con Listas

Las listas son colecciones ordenadas y mutables de elementos:

    Concatenación (+): Similar a las cadenas, unir dos listas las combina en una nueva lista.
    Repetición (*): Repite todos los elementos de la lista un número determinado de veces.

Funciones Especiales para Listas

    Zip: Toma dos o más listas y las empareja, creando una lista de tuplas. Cada tupla contiene elementos de las listas originales que ocupan la misma posición.
    Map: Aplica una función específica a cada elemento de un iterable, lo que resulta útil para transformar los datos contenidos.

Asimismo, otro de los conceptos que mencionamos es el de ‘TypeCast‘. El TypeCast, o conversión de tipo, es el proceso mediante el cual se cambia una variable de un tipo de dato a otro.

En Python, esto se realiza de manera muy directa, utilizando el nombre del tipo de dato como una función para realizar la conversión. Por ejemplo, convertir una cadena a un entero se hace pasando la cadena como argumento a la función int(), y transformar un número a una cadena se hace con la función str(). Esta capacidad de cambiar el tipo de dato es especialmente útil cuando se necesita estandarizar los tipos de datos para operaciones específicas o para cumplir con los requisitos de las estructuras de datos.

A medida que progresemos, ampliaremos nuestro repertorio para incluir operaciones más complejas y explorar otros tipos de datos y estructuras en Python.

---
## Mis apuntes: 

el primer ejemplo de operadores básicos son las sumas.
![[Pasted image 20240312120637.png]]
el segundo tipo de operacion que estaremos ocupando es la resta: 
![[Pasted image 20240312121011.png]]
la siguiente operación que vamos a realizar es la de multiplicar estos dos números y se realiza con la siguiente sentencia: 
![[Pasted image 20240312155906.png]]
  para dividir se ocupa el operador  /.
  ![[Pasted image 20240312171829.png]]
  la potencia se hace de la siguiente forma: 
  ![[Pasted image 20240312172545.png]]
  en este ejemplo podemos ver que en el print de la potencia se puede hacer un cambio del formato en donde se pone entre llaves el inicio de la condición del formato, y el replace se hacer para cambiar las comas por puntos. 

Para las listas tenemos las siguientes operaciones: 
![[Pasted image 20240312173759.png]]
la que vemos en la foto anterior es la de concatenar elementos de un array a otro, y crear un nuevo array.
ahora podemos hacer la operatoria entre elementos de cada uno de los array posicion con posicion, si se puede utilizando la funcion zip().
![[Pasted image 20240312175106.png]]
en aquí podemos ver que el zip hace tuplas de 2 elementos que están en la misma posición de ambos arreglos. 
![[Pasted image 20240312175316.png]]
con la funcion map(operacion, ) podemos hacer las operaciones de cada una de las tuplas. 
si queremos guardar estas operaciones en listas podemos ocupar la siguiente instruccion: 
![[Pasted image 20240312175714.png]]


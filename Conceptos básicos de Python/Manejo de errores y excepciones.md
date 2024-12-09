## Datos de la clase: 
En esta clase, abordaremos el manejo de errores y excepciones, un aspecto crítico para la creación de programas robustos y confiables en Python. Los errores son inevitables en la programación, pero manejarlos correctamente es lo que diferencia a un buen programa de uno que falla constantemente.

**Manejo de Errores**

Los errores pueden ocurrir por muchas razones: errores de código, datos de entrada incorrectos, problemas de conectividad, entre otros. En lugar de permitir que un programa falle con un error, Python nos proporciona herramientas para ‘atrapar’ estos errores y manejarlos de manera controlada, evitando así que el programa se detenga inesperadamente y permitiendo reaccionar de manera adecuada.

**Excepciones**

Una excepción en Python es un evento que ocurre durante la ejecución de un programa que interrumpe el flujo normal de las instrucciones del programa. Cuando el intérprete se encuentra con una situación que no puede manejar, ‘levanta’ o ‘arroja’ una excepción.

**Bloques try y except**

Para manejar las excepciones, utilizamos los bloques ‘**try**‘ y ‘**except**‘. Un bloque ‘**try**‘ contiene el código que puede producir una excepción, mientras que un bloque ‘**except**‘ captura la excepción y contiene el código que se ejecuta cuando se produce una.

**Otras Palabras Clave de Manejo de Excepciones**

- **else**: Se puede usar después de los bloques ‘**except**‘ para ejecutar código si el bloque ‘**try**‘ no generó una excepción.
- **finally**: Se utiliza para ejecutar código que debe correr independientemente de si se produjo una excepción o no, como cerrar un archivo o una conexión de red.

**Levantar Excepciones**

También es posible ‘levantar’ una excepción intencionalmente con la palabra clave ‘**raise**‘, lo que permite forzar que se produzca una excepción bajo condiciones específicas.

En esta clase, aprenderemos a identificar diferentes tipos de excepciones y cómo manejarlas de manera específica. También exploraremos cómo utilizar la declaración ‘**raise**‘ para crear excepciones que ayuden a controlar el flujo del programa y evitar estados erróneos o datos corruptos.

Al final de esta clase, tendrás las habilidades para escribir programas que manejen situaciones inesperadas de manera elegante y mantengan una ejecución limpia y controlada, incluso cuando se encuentren con problemas imprevistos.

---
## Mis apuntes: 

ahora haremos ejemplos de try/except: 
![[Pasted image 20240318183900.png]]
el primer ejemplo es intentando dividir un número por 0, esto esta claro que no se puede por lo que salta la excepción.
Algo super recomendable para los desarrolladores cuando estan tratando de utilizar este tipo de funciones, se recomienda que pongan el tipo de excepción que se requiere controlar. 
![[Pasted image 20240318184212.png]]
en este caso estamos tratando de controlar un ZeroDivisionError, por lo que ahí podemos ver que estamos controlando mejor los errores.
También tenemos un bloque que es una excepción pero que siempre se va a ejecutar: 
![[Pasted image 20240318184546.png]]
las excepciones también se pueden lanzar, esto se puede hacer gracias a la funcion raise:
![[Pasted image 20240318184824.png]]


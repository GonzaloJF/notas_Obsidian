## Datos de la clase: 

Los conceptos vistos en esta clase son esenciales para entender cómo crear programas en Python que puedan tomar decisiones y repetir acciones hasta cumplir ciertos criterios. Aquí es donde nuestros programas obtienen la capacidad de responder a diferentes situaciones y datos.

**Condicionales**

Los condicionales son estructuras de control que permiten ejecutar diferentes bloques de código dependiendo de si una o más condiciones son verdaderas o falsas. En Python, las declaraciones condicionales más comunes son ‘**if**‘, ‘**elif**‘ y ‘**else**‘.

- **if**: Evalúa si una condición es verdadera y, de ser así, ejecuta un bloque de código.
- **elif**: Abreviatura de “**else if**“, se utiliza para verificar múltiples expresiones sólo si las anteriores no son verdaderas.
- **else**: Captura cualquier caso que no haya sido capturado por las declaraciones ‘**if**‘ y ‘**elif**‘ anteriores.

**Bucles**

Los bucles permiten ejecutar un bloque de código repetidamente mientras una condición sea verdadera o para cada elemento en una secuencia. Los dos tipos principales de bucles que utilizamos en Python son ‘**for**‘ y ‘**while**‘.

- **for**: Se usa para iterar sobre una secuencia (como una lista, un diccionario, una tupla o un conjunto) y ejecutar un bloque de código para cada elemento de la secuencia.
- **while**: Ejecuta un bloque de código repetidamente mientras una condición específica se mantiene verdadera.

**Control de Flujo en Bucles**

Existen declaraciones de control de flujo que pueden modificar el comportamiento de los bucles, como ‘**break**‘, ‘**continue**‘ y ‘**pass**‘.

- **break**: Termina el bucle y pasa el control a la siguiente declaración fuera del bucle.
- **continue**: Omite el resto del código dentro del bucle y continúa con la siguiente iteración.
- **pass**: No hace nada, se utiliza como una declaración de relleno donde el código eventualmente irá, pero no ha sido escrito todavía.

En esta clase, profundizaremos en cada uno de estos aspectos con ejemplos detallados. Aprenderemos cómo tomar decisiones dentro de nuestros programas y cómo automatizar tareas repetitivas. Esto nos dará la base para escribir programas que pueden manejar tareas complejas y responder dinámicamente a los datos de entrada. Al final de la clase, estarás equipado con el conocimiento para controlar el flujo de tus programas de Python de manera eficiente y efectiva.

---
## Mis apuntes: 

### Bucles: 
Los bucles nos sirven para poder iterar dentro de un rango de números, listas, tuplas, etc.
![[Pasted image 20240318110705.png]]
En este caso podemos ver que hacemos un bucle for en un rango de 5 números que van del 0 al 4, ahora también podemos recorrer una lista: 
![[Pasted image 20240318113953.png]]
para ver y entender como hacen las iteraciones podemos hacer lo siguiente: 
![[Pasted image 20240318114147.png]]
Ahora comenzamos con el bucle while, se puede hacer los mismo que con el bucle for pero con una condicion: 
![[Pasted image 20240318115319.png]]
para los bucles for también tenemos algo llamado enumerate, esta nos devuelve 2 datos como respuesta, entre ellas el el índice y el dato que estamos consultando, como por ejemplo, lo siguiente: 
![[Pasted image 20240318124819.png]]
Ahora si queremos hacer un ejemplo con diccionarios podemos hacer lo siguiente: 
![[Pasted image 20240318140323.png]]
Ahora podemos ver los bucles anidados: 
![[Pasted image 20240318151906.png]]
podemos ver que se puede hacer un for dentro de otro for para poder llegar a acceder a elementos dentro de una lista que contiene más listas.
Ahora también podemos hacer operaciones a una lista por ejemplo podemos tener una lista con numeros impares y despues queremos rellenar una lista con el cuadrado de estos números esto se puede hacer de la siguiente forma: 
![[Pasted image 20240318152314.png]]
esto es llamado lista de comprensión, ya que estamos haciendo una operatoria en el lado izquierdo del bucle for, por lo que esta diciendo que i^2 es el valor de i.
algo que tenemos que tener en cuenta es que el orden dentro de la programación es super importante, ahora podemos hacer el siguiente ejemplo, si queremos salir antes de un bucle debido a una condicion que cumplimos podemos jugar con el condicional if y aplicar un break de la siguiente forma: 
![[Pasted image 20240318152851.png]]
otra de las condiciones que hay es la opción continue, esta nos dice que cuando se cumpla la condición no hará nada pero continuara la iteracion
![[Pasted image 20240318154311.png]]
Ahora entramos a las condiciones if y else, if es una condiciones que queremos que se nos represente o va a pasar para que se ejecute un codigo y el else es el contrario a la acción que esperamos: 
![[Pasted image 20240318163056.png]]


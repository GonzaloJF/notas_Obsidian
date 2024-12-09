## Datos de la clase: 

Esta clase se centra en una característica poderosa y expresiva de Python que permite la creación de funciones en una sola línea: las funciones lambda.

**Funciones Lambda**

Las funciones lambda son también conocidas como funciones anónimas debido a que no se les asigna un nombre explícito al definirlas. Se utilizan para crear pequeñas funciones en el lugar donde se necesitan, generalmente para una operación específica y breve. En Python, una función lambda se define con la palabra clave ‘**lambda**‘, seguida de una lista de argumentos, dos puntos y la expresión que desea evaluar y devolver.

Una de las ventajas de las funciones lambda es su simplicidad sintáctica, lo que las hace ideal para su uso en operaciones que requieren una función por un breve momento y para casos donde la definición de una función tradicional completa sería excesivamente verbosa.

**Usos comunes de las Funciones Lambda**

- **Con funciones de orden superior**: Como aquellas que requieren otra función como argumento, por ejemplo, ‘**map()**‘, ‘**filter()**‘ y ‘**sorted()**‘.
- **Operaciones simples**: Para realizar cálculos o acciones rápidas donde una función completa sería innecesariamente larga.
- **Funcionalidad en línea**: Cuando se necesita una funcionalidad simple sin la necesidad de reutilizarla en otro lugar del código.

En esta clase, aprenderemos cómo y cuándo utilizar las funciones lambda de manera efectiva, además de entender cómo pueden ayudarnos a escribir código más limpio y eficiente. Aunque su utilidad es amplia, también discutiremos las limitaciones de las funciones lambda y cómo el abuso de estas puede llevar a un código menos legible.

Al dominar las funciones lambda, ampliarás tu conjunto de herramientas de programación en Python, permitiéndote escribir código más conciso y funcional.

---
## Mis Apuntes: 

Este es un tipo de función que son más simplificadas ya que se ponen en una sola linea como en el siguiente ejemplo: 
![[Pasted image 20240318182205.png]]
ahora a este tipo de funciones anónimas también se le pueden pasar datos con variables, de la siguiente forma(se puede pasar la cantidad de numeros o variables que se quiera en la funcion lambda): 
![[Pasted image 20240318182527.png]]
Ahora para el siguiente ejemplo tenemos una lista con numeros y necesito que se eleve al cuadrado esta lista, podemos ocupar este tipo de funciones para poder hacerlo: 
![[Pasted image 20240318183123.png]]
ahora si queremos filtrar por pares o impares podemos hacer lo siguiente: 
![[Pasted image 20240318183323.png]]

## Datos de la clase: 
Python proporciona varias maneras de formatear cadenas, permitiendo insertar variables en ellas, así como controlar el espaciado, alineación y precisión de los datos mostrados. Aquí están las técnicas de formateo de cadenas que exploraremos:

**Operador % (Porcentaje)**

También conocido como “i**nterpolación de cadenas**“, este método clásico utiliza marcadores de posición como ‘**%s**‘ para cadenas, ‘**%d**‘ para enteros, o ‘**%f**‘ para números de punto flotante.

**Método format()**

Introducido en Python 2.6, permite una mayor flexibilidad y claridad. Utiliza llaves ‘**{}**‘ como marcadores de posición dentro de la cadena y puede incluir detalles sobre el formato de la salida.

**F-Strings (Literal String Interpolation)**

Disponible desde Python 3.6, los F-Strings ofrecen una forma concisa y legible de incrustar expresiones dentro de literales de cadena usando la letra ‘**f**‘ antes de las comillas de apertura y llaves para indicar dónde se insertarán las variables o expresiones.

En esta clase, nos enfocaremos en cómo utilizar cada uno de estos métodos para formatear cadenas efectivamente, así como las situaciones en las que cada uno podría ser más apropiado. Al final, tendrás las herramientas para presentar información de manera profesional en tus programas de Python.

---
## Mis apuntes: 

el primer ejemplo que queremos mostrar es el siguiente, que en realidad no tiene ningún problema por que se esta concatenando 
![[Pasted image 20240316181247.png]]
si lo ejecutamos resulta lo siguiente: 
![[Pasted image 20240316181344.png]]
otra forma de representar las variables dentro de una cadena de string: 
![[Pasted image 20240317152828.png]]
lo que muestra la consola es lo siguiente: 
![[Pasted image 20240317152856.png]]
aparte de estas tenemos otras formas de formatear un string, que es utilizando .format(): 
![[Pasted image 20240317213331.png]]
la ultima de las formas de darle formato es: 
![[Pasted image 20240318001257.png]]

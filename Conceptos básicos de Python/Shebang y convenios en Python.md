## Datos de la clase: 

En el desarrollo con Python, el shebang y los convenios de codificación son aspectos importantes que facilitan la escritura de scripts claros y portables.

**Shebang en Python:**

El shebang es una línea que se incluye al principio de un script ejecutable para indicar al sistema operativo con qué intérprete debe ejecutarse el archivo. En los scripts de Python, el shebang común es:

- **#!/usr/bin/env python3**

Esta línea le dice al sistema que utilice el entorno (**env**) para encontrar el intérprete de Python 3 y ejecutar el script con él. Es fundamental para asegurar que el script se ejecute con Python 3 en sistemas donde Python 2 todavía está presente.

**Convenios en Python:**

Los convenios de codificación son un conjunto de recomendaciones que guían a los desarrolladores de Python para escribir código claro y consistente. El más conocido es ‘**PEP 8**‘, que abarca:

- **Nombres de Variables**: Utilizar ‘**lower_case_with_underscores**‘ para nombres de variables y funciones, ‘**UPPER_CASE_WITH_UNDERSCORES**‘ para constantes, y ‘**CamelCase**‘ para clases.
- **Longitud de Línea**: Limitar las líneas a 79 caracteres para código y 72 para comentarios y docstrings.
- **Indentación**: Usar 4 espacios por nivel de indentación.
- **Espacios en Blanco**: Seguir las prácticas recomendadas sobre el uso de espacios en blanco, como no incluir espacios adicionales en listas, funciones y argumentos de funciones.
- **Importaciones**: Las importaciones deben estar en líneas separadas y agrupadas en el siguiente orden: módulos de la biblioteca estándar, módulos de terceros y luego módulos locales.
- **Compatibilidad entre Python 2 y 3**: Aunque Python 2 ha llegado al final de su vida útil, algunos convenios pueden seguirse para mantener la compatibilidad.

El cumplimiento de estos convenios no solo mejora la legibilidad del código, sino que también facilita la colaboración entre desarrolladores y el mantenimiento a largo plazo del software.

El uso adecuado del shebang y la adhesión a los convenios de Python son señales de un desarrollador cuidadoso y profesional. Integrar estos aspectos en tus prácticas de codificación es crucial para el desarrollo de software efectivo y eficiente en Python.

----
## Mis apuntes: 

para está clase haremos un ejemplo de script de python, lo primero que hay que hacer es la declaración de que se trata de un script de python esta se hace con el siguiente linea de comando: 
![[Pasted image 20240311142821.png]]
si colocamos esa linea corresponde a que queremos que la app que vamos a programar se ejecute con el inteprete de python3: 
![[Pasted image 20240311181819.png]]
hay momentos en lo que los binarios del sistema no estan en la ruta /usr/bin, por lo que una alternativa que existe es la localización ``/usr/bin/env python3 ``.
![[Pasted image 20240311190255.png]]
de la siguiente manera ejecutaremos el contenido del script: 
![[Pasted image 20240311190336.png]]
los shebangs sirven para que podamos darle permisos de ejecución al archivo en cuestión para poder ejecutarlos como un archivo cualquiera, por ejemplo: 
![[Pasted image 20240311190646.png]]
con esto si le sacamos el shebang la terminal nos mandaría una alerta por que no sabría con que interpretar el código en cuestión.
Esta es la estructura principal de un script en python: 
![[Pasted image 20240311191126.png]]
donde el if pregunda si este nombre es la estructura principal del programa.
![[Pasted image 20240311191357.png]]
esta seria una estructura de un programa de python donde puede existir un modulo pricipal y si no lo es, es por que se esta llamando desde un modulo externo, por ejemplo: 
![[Pasted image 20240311191507.png]]
este seria ejecutando el programa test.py, pero si creamos otro modulo y llamamos a este veremos lo siguiente: 
![[Pasted image 20240311191755.png]]
![[Pasted image 20240311191838.png]]
con esto podemos ver que esta llamando a la otra sentencia que no es la principal. 
bueno para las definiciones de funciones por lo general se ocupan lowerCamelCase, que es escribir la primera palabra en minúsculas para luego escribir las primeras de las siguientes palabras con mayúsculas. 
![[Pasted image 20240311192423.png]]
Para la declaracion de clases se ocupa upper camel case de la siguiente forma: 
![[Pasted image 20240312000415.png]]
podemos ver que esas son algunos convenios, formas de estructurar el programa, el screaming_snake_case, es la forma de declarar constantes dentro del programa.
por ultimo veremos unas variables protegidas y privadas: 
![[Pasted image 20240312000917.png]]

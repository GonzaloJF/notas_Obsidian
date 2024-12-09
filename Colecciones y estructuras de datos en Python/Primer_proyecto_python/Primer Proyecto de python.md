## Datos de la clase: 
En esta clase, pondremos en práctica todo lo que hemos aprendido sobre listas, tuplas, diccionarios y conjuntos para crear un sistema de gestión de videojuegos en Python. Este proyecto integrador nos servirá para consolidar nuestras habilidades de programación, ya que abordaremos la administración de ventas, la gestión de stock y la actualización de datos en un sistema de control central.

Durante la clase, te guiaremos a través del desarrollo de las funcionalidades clave de nuestro sistema, mostrando cómo se puede aplicar cada estructura de datos en situaciones concretas de manejo de información. Vamos a crear un programa que no solo administre eficientemente los datos de videojuegos, sino que también sea capaz de interactuar con el usuario para realizar consultas y actualizar el inventario.

Este enfoque práctico es ideal para afianzar los conceptos teóricos en un contexto realista, demostrando la potencia y versatilidad de Python en el desarrollo de aplicaciones. La clase será esencialmente una sesión interactiva, por lo que la mayor parte del aprendizaje se realizará a través de la observación y la práctica directa con el código en el vídeo.

Al concluir, habrás experimentado la satisfacción de construir un proyecto completo que simula un escenario del mundo real, reforzando tu comprensión de cómo las estructuras de datos se utilizan en la creación de software y preparándote para futuros proyectos de programación.

--------------------------------------------
En este primer proyecto de python jugaremos con listas, conjuntos, diccionarios y tuplas, para poder poner en practica estas estructuras: 
lo primero que haremos es separar lo que queremos hacer en nuestro programa, queremos ver las distintas características de un juego siendo nosotros dueños de una tienda.
por lo que podemos hacer lo siguiente: 
![[Pasted image 20240516134838.png]]
dentro del proyecto podemos ver los géneros de algunos juegos, ventas y stock de estos dentro de la tienda y también los clientes que han comprados dichos juegos. 
por lo que ahora nos gustaria generar un reporte de cada una de las cosas de un juego por ejemplo: 
![[Pasted image 20240516140051.png]]
creamos la variable mi_juego para saber que juego es el que estamos buscando, con esto nos aseguramos de trabajar con esa variable en cada una de las características que se quieren preguntar.
Ahora lo vamos a complicar un poco más por que estaremos trabajando con una lista para recorrerla y poder ver la información de todos los juegos: 
![[Pasted image 20240516142802.png]]
podemos ver los reportes generados en la ejecuccion: 
![[Pasted image 20240516143522.png]]
ahora podemos hacer filtros de acuerdo a la cantidad de ventas que se han realizado de algunos juegos: 
![[Pasted image 20240516143828.png]]
se puede hacer una funcion lambda para mostrar la cantidad total de ventas: 
![[Pasted image 20240516162230.png]]
ahora si queremos hacer una suma total de ventas del tope que nosotros queramos, podemos hacer lo siguiente: 
![[Pasted image 20240516174124.png]]
en este caso declaramos un tope, y podemos hacer que dentro de la funcion lambda busquemos de a cuerdo a la cantidad de ventas limitadas por el tope que nosotros busquemos. 

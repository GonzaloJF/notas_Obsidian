## Apuntes míos:
Para crear un archivo como descriptor de archivos podemos ocupar el siguiente paso: 
```
$ exec 3<> file
Para poder hacer un descriptor de archivos con acceso a lectura de ocupa el <
Para poder que el descriptor de archivos tenga acceso a escritura se ocupa >
```
Si se quiere mandar el stdout al descriptor de archivos se puede hacer de la siguiente forma: 
```
$ whoami >&3
Esto indica que enviara el output del comando a este archivo.
```
Para cerrar un descriptor de archivo podemos hacer lo siguiente: 
```
$ exec 3>&-
Esto significa que no entraran más datos a este archivo, pero no se borrará el archivo con su contenido.
```
Se puede crear un duplicado de un descriptor de archivo de la siguiente forma: 
```
$ exec 8>&3 
Con esto podemos decir que todo lo que esta en el descriptor de archivos 3 estará en el 8, asi como lo que se integre en el descriptor de archivo 8 se integrara al descriptor 3
```
Al hacer una copia se puede cerrar inmediatamente el descriptor de archivos creados anteriormente: 
```
$ exec 5>&4- 
En este ejemplo podemos ver que se puede hacer una copia del descriptor 4 en el descriptor 5 y cerrar el descriptor 4 al realizar esta acción.
```

## Elementos de  la clase S4vitar: 
Por aquí os dejo algunos recursos de apoyo para profundizar sobre el uso de descriptores de archivo en Bash:

-  Redirectores en Bash [Formato PDF]: [https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf](https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf)

Esta parte es fundamental, sobre todo de cara a las clases de scripting en Bash que tendremos más adelante, pues haremos mucho uso de estos.
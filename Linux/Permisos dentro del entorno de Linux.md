![[Pasted image 20240131112030.png]]
Como se puede ver el comando lla es una sigla para poder ocupar lsd -l,  por que si, esta instalado un plugin llamado lsd que es un ls, este nos lista los directorios y los archivos dentro de una ruta especifica.
Para hablar de permisos tenemos que ver la parte izquierda de la lista donde podemos reconocer algunas letras estas letras tienen un significado y tambien una razón de ser.
Al concentrarnos en las letras del directorio llamado gonzalo podemos identificar lo siguiente:

drwxr-xr-x 

d: Esta sigla se refiere a que es un directorio
r: Esta sigla se refiere que es un archivo/directorio de lectura(read).
w: Esta sigla nos dice que es un archivo/directorio de escritura(write).
x: Esta sigla nos dice que es un archivo/directorio ejecutable(execute).
Hay ocasiones en las que al principio se puede ver un . en vez de la d esto pasa ya que es un archivo en vez de un directorio.

<font color="red">drwx</font>|<font color="blue">r-x</font>|<font color="gren">r-x</font>  =>  <font color="red">P</font>|<font color="blue">G</font>|<font color="gren">O</font>
  

Para poder interpretar este montón de letras, hay que separarlas en 3 secciones, donde las primeras 4 letras (representadas en color rojo) son los permisos del propietario del archivo o directorio, las segundas 3 letras corresponden a los permisos que tienen los grupos sobre el archivo o directorio (representadas por el color azul), las ultimas 3 letras corresponden a los permisos que tienen el grupo final llamados otros hacia el archivo o directorio(representado por un color verde).

.rw-r--r-- gonzalo gonzalo   42 B Fri Jul 28 11:37:08 2023  file.txt

También se pueden identificar algunos datos dentro de la lista después de estas letras, como por ejemplo el primer nombre llamado gonzalo es del propietario, y el segundo nombre que no obligatoriamente será el mismo del propietario es el nombre del grupo que tiene acceso a este archivo o directorio.

  

#Ejercicio de permisos: 

1.- Nos pondremos como super usuario con el comando  sudo su.

2.- Creamos una carpeta con el comando  mkdir.

3.- Saldremos del usuario root con un exit  o con sudo gonzalo.

4.- Realizamos un lla para listar los archivos y directorios.

5.- Vemos los permisos de cada uno de los archivos y vemos que root tiene todos los permisos, pero como estamos como usuario gonzalo tenemos los permisos de otros y con esto tenemos solo los permisos de leer y ingresar al directorio.

6.- Con el comando chmod podemos cambiar los privilegios(obviamente tenemos que estar como super usuario/propietario) de las carpetas o directorios la forma de hacerlo es implementando el comando de la siguiente manera: 

chmod o+w [directorio/archivo]

7.- Como usuario gonzalo podemos ahora crear archivos implementando el comando touch  o creando carpetas con el comando mkdir.

8.- Podemos quitar los permisos a los usuarios  “otros” utilizando el mismo comando de  chamod, pero en vez de ocuparlo con un simbolo + se ocupa con un simbolo -, es decir que se escribiría: 

chmod o-w [directorio/archivos]

9.- Tambien se pueden cambiar el grupo al que esta dirigido un archivo o un directorio, el comando que debemos emplear es: 

 chgrp nombre_del_gurpo_nuevo Directorio/Archivo
  

*importante: los identificadores de los grupos para el comando chmod son:

u-> propietario

g-> grupo

o-> otros

para más informacion tenemos los siguientes enlaces: 
[https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/](https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/)

[https://atareao.es/tutorial/terminal/propietarios-y-permisos/](https://atareao.es/tutorial/terminal/propietarios-y-permisos/)


Manera octal de dar permisos: 

Hay una segunda forma en la que se pueden dar permisos a los usuarios dentro del sistema operativo de linux, esta es una forma octal, que corresponde a asignarle un numero a cada uno de los permisos dependiendo de la ubicación y si esta activo o no este permiso.

para poder hacer una forma referencial podemos hacer lo siguiente, al crear un directorio en formato root podemos ver lo siguiente 

rwx|r-x|r-x 

representado en números se puede decir que es: 

rwx|r-x|r-x 

111|101|101

Lo que debemos hacer para poder ver el código octal es ver las posiciones en las que esta escrito los 1, y elevar 2 al número que corresponda a la posición.

rwx|r-x|r-x 

111|101|101

210|210|210

2^2+2^1+2^0|2^2+0+2^0|2^2+0+2^0|

4+2+1|4+0+1|4+0+1|

7|5|5 

Entonces para darle esos permisos se podría decir de la siguiente manera con el comando: 

chmod 755 [directorio]

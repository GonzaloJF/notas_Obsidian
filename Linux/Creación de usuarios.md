  

Podemos crear usuarios simplemente con emplear el comando useradd y el nombre pero se le pueden asignar un tipo de shell y tambien un directorio personal para dicho usuario, como se puede hacer, es de la siguiente manera: 

useradd nombre -s /bin/bash -d /home/[directorio]

De esta forma ya le queda asignado un shell y un directorio, para poder darle una contraseña al usuario nuevo se puede hacer con el comando passwd que se pedirá introducir 2 veces la misma contraseña para guardarla.

Para poder cambiar el propietario se hace con el comando chown y para cambiar el grupo se hace con el comando  chgrp, o en su defecto podemos hacer las dos cosas con el comando  chown, empleándolo de la siguiente forma: 

chown nombre:nombre [directorio]

También podemos ver como manejar los grupos, tenemos el comando groupadd que nos permite poder crear un nuevo grupo.

Podemos agregar un usuario al grupo empleando el siguiente comando:

usermod -a -G grupo usuario

userdel -r nombre_del_usuario (este sirve para eliminar un usuario).

para más info:
[https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/](https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/)

[https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/](https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/)

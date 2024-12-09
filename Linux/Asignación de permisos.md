## Mis Apuntes:
Con el comando `chmod` podemos hacer la asignación de los distintos tipos de permisos estando como root, en este caso podemos hacer que sea por separado o todos a la ves: 
ejemplo de asignación de permisos: 
```
$ chmod u+wrx <File o directorio> // le estoy asignando al propietario la propiedad de escritura
$ chmod g+rwx <File o directorio> // le estoy asignando a un grupo 
$ chmod o+rwx <File o directorio> // le estoy asignando las propiedades a otros
$ chmod a+rwx <File o directorio> // le estoy dando a todos (u,g,o) las propiedades
```
para cambiarle los grupos a archivos o carpetas se puede hacer con el siguiente comando: 
`chgrp <nombre grupo> [archivo o carpeta]`
Para eliminar permisos tenemos que hacer lo siguiente: 
```
$ chmod u-x <File o directorio> // con el - se eliminan los permisos. 
```

Con `useradd` podemos crear un nuevo usuario, con el parámetro `-s` podemos asignarle el tipo de consola que queremos que tenga por ejemplo y con el parámetro `-d` le asignaremos un directorio de trabajo : 
```
$ useradd pepe -s /bin/bash -d /home/pepe
```
para asignarle una contraseña a un usuario se puede hacer con el comando `passwd` de la siguiente forma: 
```
$ passwd [usuario]
```
Para crear un nuevo grupo se puede hacer lo siguiente: 
```
$ groupadd [nombre del nuevo grupo]
```
para agregar un usuario a ese grupo se puede hacer de la siguiente manera: 
```
$ usermod -a -G [nombre del grupo] <usuario>
```
para cambiar el propietario o el grupo dentro de un archivo o directorio se hace con chown de la siguiente manera: 
```
$ chown [propietario nuevo]:<grupo nuevo> archivo o carpeta
```

## Parte de La clase: 
¿Necesitas indagar un poco más en esta temática?, te dejo por aquí algunos recursos de interés:

- Asignación de permisos: [https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/](https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/)
- Propietarios y permisos: [https://atareao.es/tutorial/terminal/propietarios-y-permisos/](https://atareao.es/tutorial/terminal/propietarios-y-permisos/)
Te comparto por aquí algunos enlaces de interés por si quieres investigar más acerca de permisos en Linux:

- Gestión de usuarios, grupos y permisos en Linux: [https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/](https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/)
- Gestión de usuarios y grupos en Linux: [https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/](https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/)
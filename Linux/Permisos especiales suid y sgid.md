Para ver los permisos Suid podemos ocupar el comando which para buscar un archivo o app además se le agregara el comando xargs para hacer un ls -l.

Esto nos ayudara a ver los permisos de ese archivo o carpeta se emplea de la siguiente manera:

which [Archivo/directorio] | xargs ls -l

Para asignarle permisos Suid ocuparemos un comando chmod  que es el siguiente: 

chmod 4755 [Directorio/Archivo] ó chmod u+s [Directorio/Archivo]

Estos permisos pueden suponer un problema ya pueden lograr comprometerlos

Para encontrar archivos con este tipo de permisos se pueden emplear el siguiente comando

find / -type f -perm -4000

find: nos permite encontrar archivos.

/: significa buscar desde la carpeta raíz.

-type f : permite filtrar en que tipo de archivo o directorio buscamos.

-perm: filtra por permiso.

-4000: busca los archivos que tengan este permiso.

SGID es lo mismo que suid solo que este trabaja con los grupos.

find / -perm -2000 2>/dev/null

Para asignar este tipo de permiso se utiliza el siguiente comando 

chmod g+s [directorio] ó chmod 2xxx [directorio]

Para más info: 
[https://deephacking.tech/permisos-sgid-suid-y-sticky-bit-linux/#:~:text=Permiso%20SGID,-El%20permiso%20SGID&text=Si%20se%20establece%20en%20un,perteneciente%2C%20el%20grupo%20del%20directorio](https://deephacking.tech/permisos-sgid-suid-y-sticky-bit-linux/#:~:text=Permiso%20SGID,-El%20permiso%20SGID&text=Si%20se%20establece%20en%20un,perteneciente%2C%20el%20grupo%20del%20directorio).

[https://www.ochobitshacenunbyte.com/2019/06/17/permisos-especiales-en-linux-sticky-bit-suid-y-sgid/](https://www.ochobitshacenunbyte.com/2019/06/17/permisos-especiales-en-linux-sticky-bit-suid-y-sgid/)

[https://www.ibiblio.org/pub/linux/docs/LuCaS/Manuales-LuCAS/SEGUNIX/unixsec-2.1-html/node56.html](https://www.ibiblio.org/pub/linux/docs/LuCaS/Manuales-LuCAS/SEGUNIX/unixsec-2.1-html/node56.html)

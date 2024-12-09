Para este ejemplo podemos ocupar un archivo nuevo o copiar un archivo.

Primero tenemos el comando lsattr este nos permite ver una lista de directorios y archivos que tienen distintos flags.

Saber de estas flags es importante a la hora de hacer pentesting ya que esto nos permitirá saber que podemos hacer con dicho archivo.

Como usuario root podemos ocupar el comando chattr este comando nos permite cambiar las flags de dichos directorios o archivos, esto se puede lograr gracias a la aplicacion del siguiente comando: 

chattr +i [directorio/archivo]

El +i corresponde a la flag inmutable, esto quiere decir que nadie tiene permitido borrar el archivo, ni siquiera el usuario root.

chattr tiene otro tipos de flags que sabemos investigar.

Para eliminar dichas flags debemos implementar el siguiente comando: 

chattr -i [directorio/archivo]

Más info en:
[https://rm-rf.es/chattr-y-lsattr-visualizar-y-modificar-atributos-en-sistemas-de-ficheros-linux/#:~:text=El%20primer%20comando%2C%20lsattr%20permite,chmod%2C%20chown%2Csetfacl%E2%80%A6](https://rm-rf.es/chattr-y-lsattr-visualizar-y-modificar-atributos-en-sistemas-de-ficheros-linux/#:~:text=El%20primer%20comando%2C%20lsattr%20permite,chmod%2C%20chown%2Csetfacl%E2%80%A6)

[https://programmerclick.com/article/5604675172/](https://programmerclick.com/article/5604675172/)


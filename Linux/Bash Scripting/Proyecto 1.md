
El primer proyecto de bash scripting es poder hacer scripting a la pagina de maquinas resueltas de savitar https://htbmachines.github.io.
partiremos ingresando a la carpeta /opt del sistema, donde haremos un touch htbmachines.sh

lo primero que haremos en este archivo es hacer una función para hacer un ctrl+c como se hace, de la siguiente manera: 

![[Pasted image 20240204163749.png]]
con el `trap ctrl_c INT` estamos haciendo que tome la funcion en cuenta al momento de ejecutarse un ctrl+c, y la funcion lo que hace es escribir un Saliendo ... con un codigo de error 1.
Ahora lo que queremos hacer es poder tomar las maquinas que tiene la pagina y poderlas tener en un archivo dentro de la carpeta opt.
esto lo podemos conseguir con el comando `curl` que es para enviar peticiones a la pagina, en este caso se hará un: 
```
$ curl -s -X GET https://htbmachines.github.com 
```
al hacer esto recibiremos algo como esto: 
![[Pasted image 20240204165252.png]]
podemos ver que es una de las viñetas nos manda a un src llamado `src="/bundle.js"` que es donde esta lo que queriamos encontrar: 
```
$ curl -s -X GET https://htbmachines.github.com/bundle.js | js-beautify
```
aquí instalaremos un paquete de npm llamado js-beautify para poder ver de mejor manera la respuesta a este curl
![[Pasted image 20240204165822.png]]
hacemos ese curl y lo mandamos al bundle.js en nuestro local. 
ahora podemos hacer un cat bundle.js para poder ver el contenido sin hacer un curl, además de que podemos hacer integrarle el js-beautify y para cambiar el contenido del archivo para que se vea siempre igual un sponge de la siguiente forma: 
```
$ cat bundle.js | js-beautify | sponge bundle.js 
```
 para hacer un menu en bash scripting se realiza los siguientes pasos: 
 ![[Pasted image 20240204170519.png]]
 Un while con las opciones "m:h" en este caso seria un m de machine y los : indican que se necesita un parámetro entregado en la consola por el usuario, h no necesita nada ya que este será el menú de ayuda 
![[Pasted image 20240204172546.png]]
hasta la primera clase hace hasta aquí, dentro de las cosas a destacar, tenemos que tener en cuenta como se declaran variables en este caso indicadores:

```
declare -i(integer) [parametro]
```
dentro del while getopts tenemos un case con distintos argumentos en este caso vamos a ocupar un parametro para capturar el nombre de la maquina a buscar dentro del archivo después.
algo a tener en cuenta es que se toma 
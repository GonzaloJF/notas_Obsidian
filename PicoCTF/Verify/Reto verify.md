en el siguiente reto pone lo siguiente:
People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate. You can download the challenge files here:

    challenge.zip
dentro de files tenemos los siguientes archivos o carpetas: 
![[Pasted image 20240830120508.png]]
tenemos que verificar si esta esa flag en esos archivos: 
![[Pasted image 20240830120603.png]]
utilizando el siguiente archivo: 
![[Pasted image 20240830120644.png]]
para esto tenemos que ocupar primero para tener todos los archivos sha256 que estan dentro de los directorios enumerados `sha256sum` de la siguiente manera: 
```
$ sha256sum files/* > hash.txt
```
de esta manera conseguiremos que todos los hash esten dentro del archivo txt, por lo que podemos hacer una comparacion despues con un greep de la siguiente manera: 
![[Pasted image 20240830120935.png]]
por lo que encontramos que si esta el hash que nos dieron al principio, por lo que ahora podemos ocupar el decrypt.sh
![[Pasted image 20240830121046.png]]


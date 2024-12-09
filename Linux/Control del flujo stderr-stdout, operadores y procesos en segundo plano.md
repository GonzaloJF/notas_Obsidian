## Apuntes míos: 
Como es sabido en la terminal de linux se pueden hacer distintos tipos de comandos, en líneas separadas pero, también se pueden crear one liners que pueden hacer 2 operaciones al mismo tiempo:
![[Pasted image 20240203112231.png]]
también tenemos el operador and `&&` este operador pareciera que hiciera lo mismo que un " ; " , pero en realidad no, ya que para que se ejecute el segundo comando el codigo de estado del primero tiene que ser exitoso: 
![[Pasted image 20240203112506.png]]
para ver los códigos de estado de los comandos ejecutados podemos ejecutar el comando `echo $?` por ejemplo: 
![[Pasted image 20240203114328.png]]
hay muchos códigos de estados de error, y de códigos exitosos generalmente es 0, pero lo importante de esto es que podemos hacer que estos errores conocidos como stderr, no se muestren, podemos ejecutar un comando con un comando siguiente que haría la redirección del stderr: 
```
$ Whoma 2>/dev/null //en este caso el número 2 corresponde al stderr, lo esta redirigiendo al archivo /dev/null
```
en caso de que esté bien el comando ejecutado la respuesta de este se llama stdout, que es el output del comando ejecutado, en el caso de que no queramos que se vea la respuesta ni de los stderr ni del stdout podemos hacer un: 
```
$ whoam || ls &>/dev/null // en este caso las barras "||" es un or, y el &> es la redireccion de ambos tipos de datos tanto el output como el error hacia el dev/null.
```
para poder ejecutar un comando en segundo plano sin que este dependa de una terminal podemos hacerlo de la siguiente manera : 
```
$ burpsuite &>/dev/null & disown // & es para dejar corriendo en segundo plano y disown es para dejarlo independiente de la terminal.
$ wireshark &>/dev/null & disown // & es para dejar corriendo en segundo plano y disown es para dejarlo independiente de la terminal.
```

## Elementos de la clase: 
Por aquí os dejo algunos recursos de apoyo para profundizar sobre el uso de redirectores en Bash:

-  Redirectores en Bash [Formato PDF]: [https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf](https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf)

Esta parte es fundamental, sobre todo de cara a las clases de scripting en Bash que tendremos más adelante, pues haremos mucho uso de estos.
## Mis Apuntes: 

primero montamos el lab instalando la iso que descargamos, despues de configurar todo, podemos ir a nuestra maquina a hacer un arp-scan: 
![[Pasted image 20240306131515.png]]
hacemos ahora un escaneo a los puertos que pueden estar abiertos dentro de la maquina victima: 
![[Pasted image 20240306131951.png]]
podemos ver que hay 3 puertos abiertos, pero ahora iremos a ver los servicios especificos y versiones: 
![[Pasted image 20240306132128.png]]
ahora si ponemos la ip en el navegador podemos ver lo siguiente: 
![[Pasted image 20240306150340.png]]
esta pagina tiene un modo de cifrado por  bloque, llamado CBC, que actua de la siguiente manera: 
![[Pasted image 20240306150708.png]]
el texto plano se cifra a través de una operación XOR con una cadena de una cierta cantidad de bits, por lo que va cifrando hasta que no se pueda descifrar sin la llave, la llave se ocupa solo en la primera operación XOR, luego de esto se va cifrando con el bloque anterior.
Para el descifrado se ocupa la llave en un comienzo y después se va descifrando con el bloque anterior: 
![[Pasted image 20240306151232.png]]
el cifrado se hace empleando un tamaño de bloque fijo, el siguiente bloque puede ser representativo de un ejemplo: 
![[Pasted image 20240306151551.png]]
Se puede emplear PKCS7, si se tiene un bloque de cierta cantidad de bits y no se completan, el relleno se hace con un carácter especifico. 
podemos ver la aritmetica que se hacer dentro del cifrado con el siguiente ejemplo: 
![[Pasted image 20240306161837.png]]
en esta imagen podemos ver como se hace la operación, entre el bloque encriptado y el bloque desencriptado anterior, en la imagen sería: 
```
C15 = E7 ^ I15
```
para ver la aritmética, la operación XOR es de acuerdo a la tabla de la verdad, donde tenemos lo siguiente: 
![[Pasted image 20240306163248.png]]
en la parte de abajo de la imagen podemos ver que los valores donde 1 - 1 y 0 - 0, son 0 y los valores alternos son 1 - 0 y 0 - 1 son iguales a 1, para devolverlos a ser decimales se tiene que elevar a su numero de acuerdo a la posición en la que este el 1, el orden de estos es de derecha a izquierda.
![[Pasted image 20240306173641.png]]
bueno con esto podemos ver que hay bloques que no quedan completos por lo que tendrá un carácter 0x1 en este caso que es el espacio en blanco del bloque.
Ahora empezamos con el ejemplo, procederemos a registrarnos: 
![[Pasted image 20240306175753.png]]
registramos y nos logueamos con nuestro usuario recién creado: 
![[Pasted image 20240306180637.png]]
si apretamos F12 y vamos a storage y podemos ver la cookie de la sesión.
ahora si queremos descifrar la cookie podemos ocupar una herramienta llamada padbuster: 
![[Pasted image 20240306181602.png]]
En la imagen podemos ver como se ocupa esta herramienta, que seria de la siguiente manera: 
```
padbuster {URL} [bloque cifrado] /*cantidad de bits del bloque*/ (otras opciones)
```
la cantidad de bits del bloque tiene que ser un multiplo de 8, ya que 8 bits equivale a 1 byte  que es la unidad minima de datos.

entonces para este ejemplo podemos ocupar de la siguiente manera el comando: 
```
padbuster http://192.168.1.82/index.php 72HCLUDcf2LRUEMbGvnIQa2Y5kzjK8kM 8 -cookies "auth=72HCLUDcf2LRUEMbGvnIQa2Y5kzjK8kM"
```
la respuesta es: 
![[Pasted image 20240306183619.png]]
![[Pasted image 20240306183636.png]]
por lo que vemos el valor de la cookie contiene user=Gonza, si ahora queremos encriptar datos podemos hacer lo siguiente: 
```
padbuster http://192.168.1.82/index.php 72HCLUDcf2LRUEMbGvnIQa2Y5kzjK8kM 8 -cookies "auth=72HCLUDcf2LRUEMbGvnIQa2Y5kzjK8kM" -plaintext 'user=admin'

```
ahora esta haciendo el caso contrario, esta tomando el texto user=admin y encriptarla, la respuesta es: 
![[Pasted image 20240306184616.png]]
si copiamos y pegamos el valor de la cookie creada, podemos ver lo siguiente: 
![[Pasted image 20240306184738.png]]
![[Pasted image 20240306184748.png]]
vemos que ahora nos ve como admin, ahora podemos hacer el ejemplo de oráculo de relleno, ya que si registramos un usuario parecido al de admin, por ejemplo bdmin, es por eso que la cookie de sesión de esta cuenta varia en una cierta cantidad de bits, por lo que a este ataque se le llama bit-flipper, ya que van a ir variando una cierta cantidad de bits en medio de la cookie para intentar descifrar la cookie del admin.
Lo que haremos en interceptar la petición get de la pagina para logeanos y mandar esta petición al intruder.
![[Pasted image 20240307140722.png]]
marcamos dentro de la cookie lo que queremos cambiar los bits, ahora vamos a payloads y cambiaremos algunas configuraciones: 
![[Pasted image 20240307141936.png]]
![[Pasted image 20240307142039.png]]
iniciamos el ataque: 
![[Pasted image 20240307142444.png]]
podemos ver que hace las cosas bien por que nos autentica como bdmin, pero no encontramos la de admin por que tiene bits extraños por que tiene caracteres especiales (%).
ahora si lo intentamos con una cuenta llamada cdmin, veremos que no tiene caracteres especiales por lo que es más fácil trabajar en este: 
![[Pasted image 20240307143100.png]]
podemos ver que tenemos una respuesta como admin.

------
## Datos de la clase: 

Un ataque de oráculo de relleno (**Padding Oracle Attack**) es un tipo de ataque contra datos cifrados que permite al atacante **descifrar** el contenido de los datos **sin conocer la clave**.

Un oráculo hace referencia a una “indicación” que brinda a un atacante información sobre si la acción que ejecuta es correcta o no. Imagina que estás jugando a un juego de mesa o de cartas con un niño: la cara se le ilumina con una gran sonrisa cuando cree que está a punto de hacer un buen movimiento. Eso es un oráculo. En tu caso, como oponente, puedes usar este oráculo para planear tu próximo movimiento según corresponda.

El relleno es un término criptográfico específico. Algunos cifrados, que son los algoritmos que se usan para cifrar los datos, funcionan en **bloques de datos** en los que cada bloque tiene un tamaño fijo. Si los datos que deseas cifrar no tienen el tamaño adecuado para rellenar los bloques, los datos se **rellenan** automáticamente hasta que lo hacen. Muchas formas de relleno requieren que este siempre esté presente, incluso si la entrada original tenía el tamaño correcto. Esto permite que el relleno siempre se quite de manera segura tras el descifrado.

Al combinar ambos elementos, una implementación de software con un oráculo de relleno revela si los datos descifrados tienen un relleno válido. El oráculo podría ser algo tan sencillo como devolver un valor que dice “Relleno no válido”, o bien algo más complicado como tomar un tiempo considerablemente diferente para procesar un bloque válido en lugar de uno no válido.

Los cifrados basados en bloques tienen otra propiedad, denominada “**modo**“, que determina la relación de los datos del primer bloque con los datos del segundo bloque, y así sucesivamente. Uno de los modos más usados es **CBC**. CBC presenta un bloque aleatorio inicial, conocido como “**vector de inicialización**” (**IV**), y combina el bloque anterior con el resultado del cifrado estático a fin de que cifrar el mismo mensaje con la misma clave no siempre genere la misma salida cifrada.

Un atacante puede usar un oráculo de relleno, en combinación con la manera de estructurar los datos de CBC, para enviar mensajes ligeramente modificados al código que expone el oráculo y seguir enviando datos hasta que el oráculo indique que son correctos. Desde esta respuesta, el atacante puede descifrar el mensaje byte a byte.

Las redes informáticas modernas son de una calidad tan alta que un atacante puede detectar diferencias muy pequeñas (menos de 0,1 ms) en el tiempo de ejecución en sistemas remotos. Las aplicaciones que suponen que un descifrado correcto solo puede ocurrir cuando no se alteran los datos pueden resultar vulnerables a ataques desde herramientas que están diseñadas para observar diferencias en el descifrado correcto e incorrecto. Si bien esta diferencia de temporalización puede ser más significativa en algunos lenguajes o bibliotecas que en otros, ahora se cree que se trata de una amenaza práctica para todos los lenguajes y las bibliotecas cuando se tiene en cuenta la respuesta de la aplicación ante el error.

Este tipo de ataque se basa en la capacidad de cambiar los datos cifrados y probar el resultado con el oráculo. La única manera de mitigar completamente el ataque es detectar los cambios en los datos cifrados y rechazar que se hagan acciones en ellos. La manera estándar de hacerlo es crear una firma para los datos y validarla antes de realizar cualquier operación. La firma debe ser verificable y el atacante no debe poder crearla; de lo contrario, podría modificar los datos cifrados y calcular una firma nueva en función de esos datos cambiados.

Un tipo común de firma adecuada se conoce como “**código de autenticación de mensajes hash con clave**” (**HMAC**). Un HMAC difiere de una suma de comprobación en que requiere una clave secreta, que solo conoce la persona que genera el HMAC y la persona que la valida. Si no se tiene esta clave, no se puede generar un HMAC correcto. Cuando recibes los datos, puedes tomar los datos cifrados, calcular de manera independiente el HMAC con la clave secreta que compartes tanto tú como el emisor y, luego, comparar el HMAC que este envía respecto del que calculaste. Esta comparación debe ser de tiempo constante; de lo contrario, habrás agregado otro oráculo detectable, permitiendo así un tipo de ataque distinto.

En resumen, para usar de manera segura los cifrados de bloques de CBC rellenados, es necesario combinarlos con un HMAC (u otra comprobación de integridad de datos) que se valide mediante una comparación de tiempo constante antes de intentar descifrar los datos. Dado que todos los mensajes modificados tardan el mismo tiempo en generar una respuesta, el ataque se evita.

El ataque de oráculo de relleno puede parecer un poco complejo de entender, ya que implica un proceso de retroalimentación para adivinar el contenido cifrado y modificar el relleno. Sin embargo, existen herramientas como **PadBuster** que pueden automatizar gran parte del proceso.

PadBuster es una herramienta diseñada para automatizar el proceso de descifrado de mensajes cifrados en modo **CBC** que utilizan relleno **PKCS #7**. La herramienta permite a los atacantes enviar peticiones HTTP con **rellenos maliciosos** para determinar si el relleno es válido o no. De esta forma, los atacantes pueden adivinar el contenido cifrado y descifrar todo el mensaje.

A continuación, se proporciona el enlace directo de descarga a la máquina ‘Padding Oracle’ de **Vulnhub**, la cual estaremos importando en VMWare para practicar esta vulnerabilidad:

- **Pentester Lab** **– Padding Oracle**: [https://www.vulnhub.com/?q=padding+oracle](https://www.vulnhub.com/?q=padding+oracle)
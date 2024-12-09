Dentro del ejemplo lo que se realizó es crear una nueva carpeta con todos los permisos.

Como usuario root le damos todos los permisos y propiedad tanto de creador como de grupo a pepe.

Al entrar en la carpeta creamos un archivo con datos o un string  esto lo hacemos con el comando echo direccionando con el signo >.

Luego como usuario gonzalo intentaremos borrar el archivo y nos daremos cuenta que si se puede, esto pasa por que la carpeta contenedora tiene todos los permisos dados.

Los permisos sticky bit sirven para denegar que cualquier usuario borre nuestros archivos como se logra esto, implementando el siguiente comando en la carpeta que aloja nuestros documentos.

chmod 1775 [Directorio]  ó  chmod +t [Directorio]

para sacar el modo sticky Bit se hace de la siguiente manera

chmod 0775 [Directorio] ó chmod -t [Directorio]

**importante: al implementar este tipo de permisos el archivo o carpeta estará funcionando constantemente por lo que el uso excesivo de este recurso puede ser contraproducente tener tantos archivos o carpetas que tengan este tipo de permiso.

para más info: 
[https://keepcoding.io/blog/que-es-el-sticky-bit-y-como-configurarlo/](https://keepcoding.io/blog/que-es-el-sticky-bit-y-como-configurarlo/)

[https://www.fpgenred.es/GNU-Linux/el_bit_sticky.html](https://www.fpgenred.es/GNU-Linux/el_bit_sticky.html)

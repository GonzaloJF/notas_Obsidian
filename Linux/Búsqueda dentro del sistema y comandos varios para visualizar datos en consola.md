  

Dentro de esto podemos ocupar el comando find en consola para poder encontrar distintos tipos de archivos.
por ejemplo podemos hacer un: 

find .|/|~ -type f -name nombre_del_archivo.

. = busca en la carpeta en la que estás trabajando actualmente (directorio actual).

/ = busca en todo el sistema.

~ = para buscar desde tu directorio home.

Este tipo de comando tiene muchos tipos de filtros en el cual la mayoría podriamos encontrarlos en internet, tenemos el siguiente url para poder investigar y tener más información. 
[https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/#La_sintaxis_basica](https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/#La_sintaxis_basica)

jugando en las consolas conectadas a una página que se llama [https://overthewire.org/](https://overthewire.org/) a través de ssh, he estado aprendiendo a ocupar mejor la busquedas con el comando find ya que te hacen trabajar mucho con este tipo de busqueda: 

uno de los comandos que más se ocupa para tener como ejemplo es: 

.- find . = busca los archivos y  directorios dentro del directorio

.- find . -type f = buscar archivos dentro del directorio

.- find . -type f -executable = busca archivos ejecutables dentro del directorio

.- find . -type f ! -executable -size 1033c = busca archivos no ejecutables dentro del directorio con un máximo peso de 1033 

.- find . -type f -readable = busca archivos legibles dentro del directorio

además cada una de estas tienen su intención ya que cada una de estas tiene más filtros que la anterior.

Además de esto estuve ocupando mucho también los xargs para poder agregar otros tipos de comandos en paralelo como serían por ejemplo cat, file(para saber que tipo de datos son), ls -la, entre otros.

Dentro de las cosas que he ocupado también está el grep  que nos permite filtrar dentro de los archivos de texto legibles.

importante, para filtrar por datos se pueden hacer de dos formas 1 es utilizando awk y la otra forma es ocupando cut ejemplos de cada uno de esto seria: 

supongamos que tenemos un archivo llamado texto.txt, en la cual tenemos una frase que dice: 

Hola soy Gonzalo, y estoy estudiando ciberseguridad.

si yo quiero que me imprima el puro nombre puedo decirle 

cat texto.txt | awk ‘{print 3}’

y para hacer esto con cut deberíamos emplear el siguiente comando: 

cat texto.txt | cut -d ‘ ’ -f 3

donde el -d sirve para marcar un delimitador y el -f sirve para indicarle que dato quieres dentro del texto.

otras de los comandos que se ocupan para poder filtrar datos sort nos sirve para poder listar las lineas que se pueden repetir y el uniq -u sirve para poder ver la linea que no se repite.

sort data.txt | uniq -u

strings sirve para poder ver las líneas legibles que se pueden ver dentro de un archivo. 
El comando base 64 nos sirve tanto para codear como decodificar datos en base 64 .
por ejemplo .
si tenemos un dato que queremos codificarlo en base 64 podemos hacer lo siguiente:

echo “hola este es un mensaje que quiero codificar ” | base64

resultado: ZXN0ZSBlcyB1biBtZW5zYWplIHF1ZSBxdWllcm8gY29kaWZpY2FyCg==

 si lo queremos decodificar se hace se la siguiente manera: 

echo "ZXN0ZSBlcyB1biBtZW5zYWplIHF1ZSBxdWllcm8gY29kaWZpY2FyCg==" | base64 --decode

la siguiente tarea viene a ser un trabajo con tr, esto nos permite traducir un algo codificado por ejemplo tenemos el siguiente texto 

Gur cnffjbeq vf WIAOOSFzMjXXBC0KoSKBbJ8puQm5lIEi

y estan rotado 13 veces es decir que la g es un t y así sucesivamente, esto podemos hacerlo fácilmente desde un navegador si buscamos rot13 y copiamos esto y lo pegamos dentro del traductor.

pero para hacerlo en codigo lo que podemos hacer es: 

cat data.txt | tr ‘[G-ZA-Fg-za-f]’ ‘[T-ZA-St-za-s]’

esto debería entregarnos la clave.

algo importante para archivos que estan en hexadecimal lo que podemos hacer es ocupar una interpretación de ellos gracias a xxd, ya que este nos permite traducir y manipular 
archivos en hexadecimal en este caso lo queremos reconvertir en un archivo legible para nosotros por lo que ocupando el comando de la siguiente forma: 

xxd -r = el -r es para indicarle que es reverse para que cambie los digitos de hexadecimal a binario.

para manejo de datos dentro de archivos, se pueden ocupar varias formas de redirigir datos a un archivo por ejemplo: 

Si queremos desviar un dato de un archivo a otro podemos ocupar el comando tee.

en cambio si queremos reemplazar los datos dentro del mismo archivo debemos ocupar sponge

manejo de conexiones ssh

para poder manejar datos de ssh tenemos que tener en cuenta que hay servicios dentro del sistema estos se controlan con el comando systemctl este es el encargado de poder activar o parar un servicio

para activar un sistema ssh lo que debemos hacer es: 

sudo systemctl start sshd

esto activara nuestro servicio ssh lo que hace que podamos hacer conecciones.

para generar las claves necesarias para establecer una conexión podemos ocupar el siguiente comando: 

ssh-keygen

esta nos dara una clave privada y una clave publica lo que nos dara la oportunidad de autorizar la entrada con esas llaves. 

para poder conectarnos con una de estas claves es con el siguiente comando: 

ssh -i [el archivo de la clave privada] algo@ip/localhost -puerto/nada.

nc  es netcat y sirve para hacer conexiones a otros servidores

ncat  es prácticamente el mismo que nc  solo que este nos ayuda con las conexiones encriptadas gracias al comando  - -ssl.

para poder conectarte a alguna maquina a traves de ssh pero que tiene bloqueada la zsh por algún motivo se puede hacer el siguiente movimiento

sshpass -p 'awhqfNnAbc1naukrpqDYcF95h7HoMTrC' ssh bandit19@bandit.labs.overthewire.org -p 2220 bash

El bash que está al final de la línea de comando es para forzar un el comando antes de la conexión así parte con ese comando desde el inicio de la máquina.

Dentro de las prácticas se utilizó netcat como una forma de poder acceder a otra contraseña de usuario de máquina virtual, en este caso es un archivo ejecutable con suid pero este nos pedia un puerto de tipo tcp para poder obtener la siguiente contraseña, para esto lo que hicimos fue:

En la kitty abrir dos ventanas ambas conectadas a la misma cuenta pero en una nos ponemos en escucha a través de netcat y en la otra ejecutamos el archivo, los comandos eran : 

nc -nlvp 4646

./suconnect 4646

Al realizar esto procederemos a entregar nuestra contraseña actual a la parte donde se esta ejecutando la netcat y con esto podemos conseguir la siguiente:

Para poder obtener otra de las contraseñas se utiliza viendo los archivos cron con esto podemos saber donde estan dejando algunos comandos ejecutarse cada cierto tiempo y que es lo que se esta dejando también 

Cron es un administrador de tareas de Linux que permite ejecutar comandos en un momento determinado, por ejemplo, cada minuto, día, semana o mes. Si queremos trabajar con cron, podemos hacerlo a través del comando crontab.
El formato de configuración de cron es el siguiente: Minuto Hora Dia-del-Mes Mes Dia-de-la-Semana Comando-a-Ejecutar
El intervalo de tiempo se especifica mediante 5 campos que representan, de izquierda a derecha:

- Minutos: de 0 a 59.
    
- Horas: de 0 a 23.
    
- Día del mes: de 1 a 31.
    
- Mes: de 1 a 12.
    
- Día de la semana: de 1 a 6 lunes a sábado (1=lunes, 2=martes, etc.) y 0 o 7 el domingo.
    
Si quisiéramos especificar todos los valores posibles de un parámetro (minutos, horas, etc.) podemos hacer uso del asterisco (*). Esto implica que si en lugar de un número utilizamos un asterisco, el comando indicado se ejecutará cada minuto, hora, día de mes, mes o día de la semana, como en el siguiente ejemplo:

* * * * * /home/user/script.sh
Para poder entender un poco más de cron con ejemplos y de crontab [https://blog.desdelinux.net/cron-crontab-explicados/](https://blog.desdelinux.net/cron-crontab-explicados/)
 
```
AWK ‘NR==3’ > indica lo que esta en la linea 3, si cambia el numero cambia la linea a imprimir
```

## Aportes de la clase:
Te dejo por aquí la guía de mi canal de Youtube en caso de que quieras pasarte a Arch Linux, recuerda que es totalmente opcional:

- Tutorial para configurar Arch Linux desde cero: [https://www.youtube.com/watch?v=fshLf6u8B-w](https://www.youtube.com/watch?v=fshLf6u8B-w)

Adicionalmente, te dejo a continuación una guía para que puedas profundizar un poco más acerca del comando ‘**find**‘:

-  Comandos Find y Locate en Linux: [https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/](https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/)
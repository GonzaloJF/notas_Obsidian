## Mis apuntes: 

para este ejemplo ocuparemos laboratorios de este github: 
https://github.com/blabla1337/skf-labs

Dentro del directorio skf-labs, iremos a python, CSTI, y ejecutaremos el programa CSTI.py de la siguiente manera: 
```
python3 CSTI.py
```
el cual nos montará un servidor local en el puerto 5000.
![[Pasted image 20240306020120.png]]
si entramos en el localhost:5000 veremos la siguiente pagina: 
![[Pasted image 20240306020154.png]]
podemos ver payloads a cargar en payloadsallthethisngs, https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md
ese es especificamente.
![[Pasted image 20240306020800.png]]

-------
## Datos de la clase: 

El **Client-Side Template Injection** (**CSTI**) es una vulnerabilidad de seguridad en la que un atacante puede inyectar código malicioso en una **plantilla de cliente**, que se ejecuta en el **navegador** del usuario en lugar del servidor.

A diferencia del **Server-Side Template Injection** (**SSTI**), en el que la plantilla de servidor se ejecuta en el servidor y es responsable de generar el contenido dinámico, en el CSTI, la plantilla de cliente se ejecuta en el navegador del usuario y se utiliza para generar contenido dinámico en el lado del cliente.

Los atacantes pueden aprovechar una vulnerabilidad de CSTI para inyectar código malicioso en una plantilla de cliente, lo que les permite ejecutar comandos en el navegador del usuario y obtener acceso no autorizado a la aplicación web y a los datos sensibles.

Por ejemplo, imagina que una aplicación web utiliza plantillas de cliente para generar contenido dinámico. Un atacante podría aprovechar una vulnerabilidad de CSTI para inyectar código malicioso en la plantilla de cliente, lo que permitiría al atacante ejecutar comandos en el navegador del usuario y obtener acceso no autorizado a los datos sensibles de la aplicación web.

Una derivación común en un ataque de Client-Side Template Injection (CSTI) es aprovecharlo para realizar un ataque de **Cross-Site Scripting** (**XSS**).

Una vez que un atacante ha inyectado código malicioso en la plantilla de cliente, puede manipular los datos que se muestran al usuario, lo que le permite ejecutar código JavaScript en el navegador del usuario. A través de este código malicioso, el atacante puede intentar robar la cookie de sesión del usuario, lo que le permitiría obtener acceso no autorizado a la cuenta del usuario y realizar acciones maliciosas en su nombre.

Para prevenir los ataques de CSTI, los desarrolladores de aplicaciones web deben validar y filtrar adecuadamente la entrada del usuario y utilizar herramientas y frameworks de plantillas seguros que implementen medidas de seguridad para prevenir la inyección de código malicioso.
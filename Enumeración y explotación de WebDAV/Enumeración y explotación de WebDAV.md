## Datos de la clase: 

**WebDAV** (**Web Distributed Authoring and Versioning**) es una extensión del protocolo HTTP que permite a los usuarios **acceder** y **manipular** **archivos** en un servidor web a través de una conexión segura.

Cuando hablamos de enumerar un servidor WebDAV, a lo que nos referimos es al proceso de recopilar información sobre los recursos disponibles en el servidor WebDAV. Los atacantes pueden utilizar herramientas de enumeración de WebDAV para buscar recursos protegidos en el servidor, como archivos de configuración, contraseñas y otros datos confidenciales. Los atacantes pueden utilizar la información recopilada durante la enumeración para planificar ataques más sofisticados contra el servidor.

Asimismo, esta fase inicial de reconocimiento implica el intentar identificar las **extensiones de archivo** permitidas en el servidor. Una vez detectadas las extensiones de archivo permitidas, los atacantes pueden aprovecharse de esto para cargar y ejecutar archivos que contengan código malicioso. Si los archivos maliciosos se cargan y ejecutan con éxito en el servidor web, los atacantes pueden obtener acceso no autorizado al servidor y comprometer la seguridad del sistema.

Una de las herramientas que vemos en esta clase es **Davtest**. La herramienta Davtest es una herramienta de línea de comandos que se utiliza para realizar pruebas de penetración en servidores WebDAV. Davtest puede utilizarse para enumerar recursos protegidos en un servidor WebDAV, así como para probar la configuración de seguridad del servidor. Davtest también puede utilizarse para probar la autenticación y la autorización del servidor, y para detectar vulnerabilidades conocidas.

Otra de las herramientas que vemos en esta clase es **Cadaver**. Cadaver es otra herramienta de línea de comandos que se utiliza para interactuar con servidores WebDAV. Cadaver permite a los usuarios navegar por los recursos del servidor, cargar y descargar archivos, y ejecutar comandos en el servidor. Cadaver también puede utilizarse para realizar pruebas de penetración en servidores WebDAV, como la enumeración de recursos protegidos y la explotación de vulnerabilidades conocidas.

Para prevenir la enumeración y explotación de WebDAV, es importante que los administradores de sistemas implementen medidas de seguridad adecuadas en el servidor. Esto puede incluir la limitación de los recursos disponibles en el servidor y la utilización de autenticación y autorización fuertes. Además, es importante que los usuarios protejan sus contraseñas y eviten el uso de contraseñas débiles o fáciles de adivinar.

A continuación, se proporciona el enlace al proyecto de Github el cual estaremos usando en esta clase para desplegar un entorno vulnerable con el que poder practicar:

- **WebDav**: [https://hub.docker.com/r/bytemark/webdav](https://hub.docker.com/r/bytemark/webdav)

-------
## Mis apuntes: 

Para esto montaremos un laboratorio que esta al final de los datos de la clase, dentro de la pagina nos saldrá como desplegar este lab. primero copiamos este comando `docker pull bytemark/webdav`
Para luego hacer lo siguiente  
```
docker run --restart always -v /srv/dav:/var/lib/dav -e AUTH_TYPE=Digest -e USERNAME=admin -e PASSWORD=peanut --publish 80:80 -d bytemark/webdav
```
luego de esto podemos empezar a trabajar, si vamos al local host podemos ver lo siguiente: 
![[Pasted image 20240523125135.png]]
En este caso nos sale un formulario de ingreso a la página, por lo que en un caso real podriamos no saber las credenciales, una de las formas de saber a que nos enfrentamos es utilizando la herramienta "whatweb", en este caso nos da la siguiente informacion: 
![[Pasted image 20240523125500.png]]
Otra herramienta que nos servirá mucho para ver que tipo de contenido funciona o se puede subir a estos webdavs, es **davtest** está herramienta funciona proporcionandole el paramerto url y pasandole la -url donde se aloja el webdav además de con el parametro -auth le tenemos que proporcionar el nombre de usuario y la contraseña de la siguiente forma.
![[Pasted image 20240523125937.png]]
en la imagen siguiente veremos como saldrá en el caso de que la clave o el usuario esté incorrecto: 
![[Pasted image 20240523130057.png]]
y en las siguientes imagenes veremos como aparecera en el caso de que estén bien proporcionada las credenciales.
![[Pasted image 20240523130217.png]]
![[Pasted image 20240523130231.png]]
También podemos ver con el siguiente oneliner cual es la contraseña sin necesidad de utilizar hydra, es utilizando la libreria rockyou.txt. 
```
cat /usr/share/wordlists/rockyou.txt | while read password; do  response=$(davtest -url http://127.0.0.1 -auth admin:$password 2>&1 | grep -i succeed); if [ $response ]; then echo " [+] La contraseña conrrecta es $password"; break; fi; done 
```
en este caso podemos ver que leemos la libreria rockyou.txt, esto lo pasaremos a un parametro password, esta la pondremos más adelante en la respuesta de davtest con lo hecho anteriormente, y utilizamos greep para cuando salga algun succeed dentro de la respuesta se detenga nos muestre la contraseña y finalice la tarea. 
![[Pasted image 20240523141037.png]]
otra herramienta super buena para trabajar con webdav es cadaver, si no tenemos instalada esta herramienta podemos hacer un `apt install cadaver`.
![[Pasted image 20240523150426.png]]
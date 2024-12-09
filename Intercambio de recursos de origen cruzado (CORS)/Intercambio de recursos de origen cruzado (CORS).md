## Datos de la clase: 
El **Intercambio de recursos de origen cruzado** (**CORS**) es un mecanismo que permite que un servidor web **restrinja** el **acceso a recursos** de **diferentes orígenes**, es decir, de diferentes dominios o protocolos. CORS se utiliza para proteger la privacidad y seguridad de los usuarios al evitar que otros sitios web accedan a información confidencial sin permiso.

Supongamos que tenemos una aplicación web en el dominio “**example.com**” que utiliza una API web en el dominio “**api.example.com**” para recuperar datos. Si la aplicación web está correctamente configurada para CORS, solo permitirá solicitudes de origen cruzado desde el dominio “**example.com**” a la API en el dominio “**api.example.com**“. Si se realiza una solicitud desde un dominio diferente, como “**attacker.com**“, la solicitud será bloqueada por el navegador web.

Sin embargo, si la aplicación web no está correctamente configurada para CORS, un atacante podría aprovecharse de esta debilidad para acceder a recursos y datos confidenciales. Por ejemplo, si la aplicación web no valida la autorización del usuario para acceder a los recursos, un atacante podría inyectar código malicioso en una página web para realizar solicitudes a la API de la aplicación en el dominio “**api.example.com**“.

El atacante podría utilizar herramientas automatizadas para probar diferentes valores de encabezados CORS y encontrar una configuración incorrecta que permita la solicitud desde otro dominio. Si el atacante tiene éxito, podría acceder a recursos y datos confidenciales que no deberían estar disponibles desde su sitio web. Por ejemplo, podría recuperar la información de inicio de sesión de los usuarios, modificar los datos de la aplicación, etc.

Para prevenir este tipo de ataque, es importante configurar adecuadamente CORS en la aplicación web y asegurarse de que solo se permitan solicitudes de origen cruzado desde dominios confiables.

-----
## Mis apuntes:
para esto ocuparemos un laboratorio de skf-labs en este caso montaremos el siguiente script de docker: 
```nginx
docker pull blabla1337/owasp-skf-lab:cors
```
para despues arrancar el docker de la siguiente forma: 
```nginx
docker run -dit -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:cors
```
Tenemos la siguiente 
![[Pasted image 20240529160302.png]]
la idea del cors es poder cambiar el origen de donde vienen los recursos a la pagina lo veremos con un ejemplo: 
![[Pasted image 20240603233750.png]]
como podemos ver tenemos un access-control-allow-origin, que es el origen de donde esta recibiendo los datos. 
![[Pasted image 20240603233928.png]]
si vemos que cambiamos el apartado de Origin en el request en el response obtenemos la pagina como ACAO.
por lo que ahora queremos obtener toda la información que tiene la pagina una vez nos autentificamos. 
![[Pasted image 20240603234454.png]]
montamos un servidor en el puerto 80, además haremos un archivo llamado malicious.html donde hasta el momento pondremos solo etiquetas de script. 
pero montaremos un script para poder obtener toda la información que tiene el administrador.

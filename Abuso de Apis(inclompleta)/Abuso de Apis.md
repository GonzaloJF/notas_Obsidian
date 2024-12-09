## Datos de la clase: 
**Actualización 24/05/2023**: Si a la hora de desplegar el laboratorio con Docker, os encontráis con problemas y alguno de los contenedores que se despliegan véis que causan error, probad a desplegar como alternativa el laboratorio de desarrollo.

Primeramente instalad la última versión de ‘**docker-compose**‘ y una vez hecho, ejecutad los siguientes comandos:

- **curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/develop/deploy/docker/docker-compose.yml**
- **VERSION=develop docker-compose pull**
- **VERSION=develop docker-compose -f docker-compose.yml –compatibility up -d**

En caso de que veáis que tras desplegar el laboratorio, siguen habiendo errores en el despliegue de ciertos contenedores, probad a hacer un ‘**docker rm $(docker ps -a -q) –force**‘ y aplicad el último comando de los 3 mencionados anteriormente para volver a desplegar los contenedores. Llegará un momento en el que todos serán desplegados sin ningún problema.

Por otro lado, si de pronto véis que el comando ‘**docker rm $(docker ps -a -q) –force**‘ os da algún problema, esperad unos segundos y volved a probar el comando hasta que veáis que todos los contenedores han sido eliminados.

Cuando hablamos del abuso de APIs, a lo que nos referimos es a la explotación de vulnerabilidades en las interfaces de programación de aplicaciones (**APIs**) que se utilizan para permitir la comunicación y el intercambio de datos entre diferentes aplicaciones y servicios en una red.

Un ejemplo sencillo de API podría ser la integración de Google Maps en una aplicación de transporte. Imagina que una aplicación de transporte necesita mostrar el mapa y la ruta a seguir para que los usuarios puedan ver la ubicación del vehículo y el camino que se va a seguir para llegar a su destino. En lugar de crear su propio mapa, la aplicación podría utilizar la API de Google Maps para mostrar el mapa en su aplicación.

En este ejemplo, la API de Google Maps proporciona una serie de funciones y protocolos que permiten a la aplicación de transporte comunicarse con los servidores de Google y acceder a los datos necesarios para mostrar el mapa y la ruta. La API de Google Maps también maneja la complejidad de mostrar el mapa y la ruta en diferentes dispositivos y navegadores, lo que permite a la aplicación de transporte centrarse en su funcionalidad principal.

Adicionalmente, una de las utilidades que vemos en esta clase es **Postman**. Postman es una herramienta muy popular utilizada para probar y depurar APIs. Con Postman, los desarrolladores pueden enviar solicitudes a diferentes endpoints y ver las respuestas para verificar que la API está funcionando correctamente. Sin embargo, los atacantes también pueden utilizar Postman para explorar los endpoints de una API en busca de vulnerabilidades y debilidades de seguridad.

Algunos endpoints de una API pueden aceptar diferentes métodos de solicitud, como GET, POST, PUT, DELETE, etc. Los atacantes pueden utilizar herramientas de fuzzing para enviar una gran cantidad de solicitudes a un endpoint en busca de vulnerabilidades. Por ejemplo, un atacante podría enviar solicitudes GET a un endpoint para enumerar todos los recursos disponibles, o enviar solicitudes POST para agregar o modificar datos.

Algunas de las vulnerabilidades comunes que se pueden explotar a través del abuso de APIs incluyen:

- **Inyección de SQL**: los atacantes pueden enviar datos maliciosos en las solicitudes para intentar inyectar código SQL en la base de datos subyacente.
- **Falsificación de solicitudes entre sitios (CSRF)**: los atacantes pueden enviar solicitudes maliciosas a una API en nombre de un usuario autenticado para realizar acciones no autorizadas.
- **Exposición de información confidencial**: los atacantes pueden explorar los endpoints de una API para obtener información confidencial, como claves de API, contraseñas y nombres de usuario.

Para evitar el abuso de APIs, los desarrolladores deben asegurarse de que la API esté diseñada de manera segura y que se validen y autentiquen adecuadamente todas las solicitudes entrantes. También es importante utilizar cifrado y autenticación fuertes para proteger los datos que se transmiten a través de la API.

Los desarrolladores pueden utilizar herramientas como Postman para probar la API y detectar posibles vulnerabilidades antes de que sean explotadas por los atacantes.

A continuación, se proporciona el enlace al proyecto de Github que utilizamos para desplegar con Docker el laboratorio vulnerable donde poder practicar la enumeración de APIs:

- **crAPI**: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)
---
## Mis Apuntes: 

Para este ejemplo tendremos que actualizar el docker-compose, esto se hace gracias al siguiente curl: 

```
curl -L "https://github.com/docker/compose/releases/download/v2.24.7/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose
```

luego de esto tenemos que ir a: 
```
cd /usr/local/bin/
para hacer un: 
mv /home/gonza/Academia/Api/crAPI/docker-compose .
```

luego de esto vamos a una carpeta hacemos un git clone del ejemplo que queremos ocupar ahora: 
```
git clone https://github.com/OWASP/crAPI
```
luego de eso ingresaremos a las carpetas: 
```
cd crAPI
cd deploy
cd docker
```
dentro de este haremos los siguientes comandos: 
```
docker-compose pull
docker-compose -f docker-compose.yml --compatibility up -d
```
y con esto estaremos con los contenedores listos para hacer el ejemplo que queremos realizar: 
![[Pasted image 20240321225315.png]]
El que nos interesa es la pagina web que esta alojada en el localhost:8888.
![[Pasted image 20240321225455.png]]
está es la pagina que tenemos que ver al visitar la web localhost:8888. 
si apretamos`` "<F12>"`` podremos desplegar el panel donde se pueden tener la consola, las cookies, entre otros tipos de datos, para poder ver las consultas a la api. 
![[Pasted image 20240322001009.png]]
Ahora ingresamos a la cuenta que nos creamos recién donde las credenciales que estamos utilizando son: `email: gonzalo@gonza.com | contraseña: Gonzalo1!$`
![[Pasted image 20240322003020.png]]
podemos ver todas las peticiones get que se hacen. 
Si vemos la peticion del login podremos ver lo siguiente: 
![[Pasted image 20240322003501.png]]
el post con la url se llama endpoint, es la url de la api para poder obtener su respuesta, si vamos al request podemos ver la estructura en json de los datos tramitados en esta peticion: 
![[Pasted image 20240322003606.png]]
si vamos a response podemos ver la respuesta del servidor: 
![[Pasted image 20240322004150.png]]
dentro de esta nos dan un token, que esta creado con jwt.io, y es un token de tipo bearer. 
ahora para trabajar de manera más ordenada vamos a ocupar la siguiente herramienta: https://techbear.co/install-postman-debian-linux/
ahora abrimos postman, para poder hacer consultas a la api. 
primero tenemos que crear una nueva colección, para poder ir guardando todas nuestras request. 
![[Pasted image 20240322120614.png]]
luego de eso le damos a hacer un nuevo http request. 
![[Pasted image 20240322120711.png]]
entonces ahora lo que se hace es cambiar el tipo de request que estamos haciendo como queremos representar el login, lo que tenemos que hacer es, cambiar de get a post y copiar el link del endpoint de la request, pero si lanzamos esta peticion asi no pasara nada por que faltan los datos en los campos correspondientes entonces tenemos que ir a la consola del navegador y copiar y pegar los datos de la siguiente manera: 
![[Pasted image 20240322120922.png]]
podemos ver que vamos al body y pinchamos en la opcion raw, para luego pegar los datos en json.
![[Pasted image 20240322125646.png]]
si apretamos send podemos ver que nos entrega como respuesta el token.
ahora se puede trabajar con variables para poder arrastrar el token que nos entrega la pagina. 
![[Pasted image 20240322131150.png]]
en la colección creamos una variable en la cual inicializamos con dos  -- y en el current value, le pasamos el token que conseguimos con el post anterior, para luego ir a authorization para poner el tipo de token y pasarle la variable creada. 
![[Pasted image 20240322131315.png]]
y ahora al hacer una petición get a la api del dashboard veremos lo siguiente: 
![[Pasted image 20240322131444.png]]
nos muestra la información que tenemos. 
![[Pasted image 20240322131550.png]]
ahora enumeraremos el shop en postman, podemos ver el endpoint esta en get así que no hay que pasarle nada en el cuerpo de la petición. 
![[Pasted image 20240322133259.png]]


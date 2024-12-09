## Datos de la clase: 
La vulnerabilidad de redirección abierta, también conocida como **Open Redirect**, es una vulnerabilidad común en aplicaciones web que puede ser explotada por los atacantes para dirigir a los usuarios a sitios web maliciosos. Esta vulnerabilidad se produce cuando una aplicación web permite a los atacantes manipular la URL de una página de redireccionamiento para redirigir al usuario a un sitio web malicioso.

Por ejemplo, supongamos que una aplicación web utiliza un parámetro de redireccionamiento en una URL para dirigir al usuario a una página externa después de que se haya autenticado. Si esta URL no valida adecuadamente el parámetro de redireccionamiento y permite a los atacantes modificarlo, los atacantes pueden dirigir al usuario a un sitio web malicioso, en lugar del sitio web legítimo.

Un ejemplo de cómo los atacantes pueden explotar la vulnerabilidad de redirección abierta es mediante la creación de correos electrónicos de **phishing** que parecen legítimos, pero que en realidad contienen enlaces manipulados que redirigen a los usuarios a un sitio web malicioso. Los atacantes pueden utilizar técnicas de **ingeniería social** para convencer al usuario de que haga clic en el enlace, como ofrecer una oferta atractiva o una oportunidad única.

Para prevenir la vulnerabilidad de redirección abierta, es importante que los desarrolladores implementen medidas de seguridad adecuadas en su código, como la validación de las URL de redireccionamiento y la limitación de las opciones de redireccionamiento a sitios web legítimos. Los desarrolladores también pueden utilizar técnicas de codificación segura para evitar la manipulación de URL, como la codificación de caracteres especiales y la eliminación de caracteres no válidos.

A continuación, se proporcionan los enlaces a los 3 proyectos de Github que estaremos desplegando en esta clase para practicar esta vulnerabilidad en un entorno controlado, viendo diferentes casos e incluso técnicas para evadir posibles restricciones:

- **Open Redirect 1**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection)
- **Open Redirect 2**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder)
- **Open Redirect 3**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2)

----
## Mis apuntes: 

Para esta vulnerabilidad podremos encontrar 3 escenarios distintos, de los cuales, estan en skf-labs/nodejs/url-redirection es el primer caso.
estando dentro de esta carpeta haremos un npm install y un npm start. 
![[Pasted image 20240522155137.png]]
podremos ver que se monta esta pagina, donde se muestra como si la pagina fuera antigua y esta mala la redirección.
si vemos la pagina bien tenemos un boton que nos lleva a un nuevo sitio web. 
![[Pasted image 20240522155303.png]]
si lo presionamos vemos que nos redirige a otra pagina web. 
si interceptamos la peticion de redireccion podemos ver lo siguiente: 
![[Pasted image 20240522155629.png]]
que tiene como post un `/redirect?newurl=/newsite`, esto quiere decir que esta haciendo una redireccion de este modo, lo cual nosotros podriamos alterar de la siguiente manera: 
`http://localhost:5000/redirect?newurl=https://www.google.com`
si colocamos esta url en el buscador veremos que nos saca de la pagina principal:
![[Pasted image 20240522155912.png]]
pero podemos ver que es el servidor el que hace la peticion para entrar a esta pagina, lo que se podria pensar que es una tonteria de vulnerabilidad pero con esto se pueden montar bots nets, y poder hacer redireccionamientos a distintas paginas con miles de servidores al mismo tiempo. 

-Segundo ejemplo: /skf-labs/nodejs/url-redirection-harder. 
una vez dentro del directiorio hacemos un npm install y un npm start. 
hacemos un localhost:5000, tenemos la pagina.
![[Pasted image 20240522170448.png]]
Si interceptamos la peticion podemos ver lo siguiente: 
![[Pasted image 20240522175852.png]]
es practicamente lo mismo que el ejemplo anterior pero podemos ver que si lo utilizamos para redirigir  a otra pagina podemos ver que nos bloquea el punto. 
![[Pasted image 20240522181417.png]]
por lo que podemos hacer es url encodear el punto en este caso seria `%2e`.
![[Pasted image 20240522182004.png]]
pero en este caso el navegador sigue tomandolo como un punto, lo que podemos hacer es url-encodear el % que seria `%25`.
![[Pasted image 20240522182354.png]]
es ahí donde si nos redirige a google.
si hacemos el tercer ejemplo es más de lo mismo solo que ahora cambia la restriccion. 
![[Pasted image 20240522200049.png]]
volvemos a la misma pagina, podemos interceptarla petición. 
![[Pasted image 20240522201254.png]]
ahora la restricción es el punto y las  barras "/", en este caso podemos simplemente borrar las barras y funcionará. 
![[Pasted image 20240522201356.png]]

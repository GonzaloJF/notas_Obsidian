## Datos de la clase: 

Los ataques de transferencia de zona, también conocidos como ataques **AXFR**, son un tipo de ataque que se dirige a los servidores **DNS** (**Domain Name System**) y que permite a los atacantes obtener información sensible sobre los dominios de una organización.

En términos simples, los servidores DNS se encargan de traducir los nombres de dominio legibles por humanos en direcciones IP utilizables por las máquinas. Los ataques AXFR permiten a los atacantes obtener información sobre los registros DNS almacenados en un servidor DNS.

El ataque AXFR se lleva a cabo enviando una solicitud de transferencia de zona desde un servidor DNS falso a un servidor DNS legítimo. Esta solicitud se realiza utilizando el protocolo de transferencia de zona DNS (AXFR), que es utilizado por los servidores DNS para transferir registros DNS de un servidor a otro.

Si el servidor DNS legítimo no está configurado correctamente, puede responder a la solicitud de transferencia de zona y proporcionar al atacante información detallada sobre los registros DNS almacenados en el servidor. Esto incluye información como los nombres de dominio, direcciones IP, servidores de correo electrónico y otra información sensible que puede ser utilizada en futuros ataques.

Una de las herramientas que utilizamos en esta clase para explotar el AXFR es **dig**. El comando dig es una herramienta de línea de comandos que se utiliza para realizar consultas DNS y obtener información sobre los registros DNS de un dominio específico.

La sintaxis para aplicar el AXFR en un servidor DNS es la siguiente:

➜ `dig @<DNS-server> <domain-name> AXFR`

Donde:

 DNS-server:es la dirección IP del servidor DNS que se desea consultar.
 domain-name: es el nombre de dominio del cual se desea obtener la transferencia de zona.
AXFR: es el tipo de consulta que se desea realizar, que indica al servidor DNS que se desea una transferencia de zona completa.

Para prevenir los ataques AXFR, es importante que los administradores de red configuren adecuadamente los servidores DNS y limiten el acceso a la transferencia de zona solo a servidores autorizados. También es importante mantener actualizado el software del servidor DNS y utilizar técnicas de cifrado y autenticación fuertes para proteger los datos que se transmiten a través de la red. Los administradores también pueden utilizar herramientas de monitoreo de DNS para detectar y prevenir posibles ataques de transferencia de zona.

A continuación, se proporciona el enlace correspondiente al proyecto de Github que nos descargamos para desplegar un entorno en Docker vulnerable con el que poder practicar:

- **DNS-Zone-Transfer**: [https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer](https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer)

---------
## Mis apuntes: 

Este tipo de ataques lo que se intenta hacer es poder sacar información sobre los dominios, subdominios o información importante de los servidores donde alojan los servicios las distintas empresas. 

para hacer este ejemplo haremos un laboratorio, en el cual tendremos que cambiar algunas configuraciones, como por ejemplo: 
tenemos el siguiente archivo llamado `named.conf.local`,  donde encontraremos la siguiente informacion: 
![[Pasted image 20240521185840.png]]
en este caso pondemos ver el nombre del dominio y donde apunta el archivo de rutas, si vamos al archivo vulhub.db podemos ver lo siguiente: 
![[Pasted image 20240521185948.png]]
podemos ver información del dominio y de los subdominios que contemplan esta corporación, por lo que si hacemos una solicitud de cambio de zona y recibimos una respuesta positiva puede ser critico por que podríamos estar recibiendo toda la información de los dominios y subdominios de la empresa y evitaríamos hacer la búsqueda por fuerza bruta con gobuster o wfuzz. 

Después de hacer estos cambios a la configuración lo que procederemos a hacer es ocupar docker-compose para arrancar el laboratorio. 
`docker-compose up -d`

ahora que levantamos el laboratorio ocuparemos una herramienta llamada `dig`.
en el primer ejemplo lo que haremos es buscar los name servers con el diminutivo `ns` empleándolo de la siguiente manera: 
![[Pasted image 20240521194518.png]]
también podemos buscar información sobre correos con el diminutivo `mx`, o como buscamos hacer una petición de cambio de zona podemos hacer lo siguiente. 
![[Pasted image 20240521194729.png]]
ocupar `dig axfr @ip "dominio"`con esto podemos saber la informacion sobre el dominio y los subdominios de la corporacion.


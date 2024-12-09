Nmap tiene una cantidad muy grande de scripts podemos buscar los scripts con el comando `locate` de la siguiente forma: 
```
$ locate .nse //La extención .nse es de los scripts de Lua
```
![[Pasted image 20240206001626.png]]
primero y muy importante es poder hacer un -sC dentro del comando de nmap ya que ese parámetro nos indica que queremos lanzar unos cuantos scripts de reconocimiento al puerto para poder saber ante que estamos presentes, como ejemplo ocuparemos: 
```
$ nmap -p22 -sCV 192.168.1.1 
```
![[Pasted image 20240206015352.png]]
estos son 2 de los scripts más ocupados dentro del parámetro -sC: 
![[Pasted image 20240206015835.png]]
![[Pasted image 20240206015913.png]]
cada uno de estos scripts tiene categorias, para poder verlas podemos hacer el siguiente comando: 
```
$ locate .nse | xargs grep "categories" //xargs nos permite poder hacer funcionar 2 comandos en paralelo.
```
![[Pasted image 20240206020442.png]]
entonces tenemos algunas categorías como "safe", "discovery", "broadcast", entre otros. 
Bueno para saber cuales son las categorías que existen podemos seguir complementando el comando anteriormente:
![[Pasted image 20240206020929.png]]
esas serian todas las categorías de los scripts.
para poder ocuparlos podemos hacerlo introduciendo el comando --script="algo" por ejemplo: 
```
$ nmap --script="vuln and safe" <ip> // esto hará que se lancen scripts que contengan ambos tipos de categorias al mismo tiempo
```
uno de los script para ver el contenido de ciertas paginas es el http-enum y se ocupa así: 
```
$ nmap -p80 <ip> --script http-enum
```

----
## Info de la clase: 


Una de las características más poderosas de Nmap es su capacidad para automatizar tareas utilizando scripts personalizados. Los scripts de Nmap permiten a los profesionales de seguridad automatizar las tareas de reconocimiento y descubrimiento en la red, además de obtener información valiosa sobre los sistemas y servicios que se están ejecutando en ellos. El parámetro –script de Nmap permite al usuario seleccionar un conjunto de scripts para ejecutar en un objetivo de escaneo específico.

Existen diferentes categorías de scripts disponibles en Nmap, cada una diseñada para realizar una tarea específica. Algunas de las categorías más comunes incluyen:

    default: Esta es la categoría predeterminada en Nmap, que incluye una gran cantidad de scripts de reconocimiento básicos y útiles para la mayoría de los escaneos.
    discovery: Esta categoría se enfoca en descubrir información sobre la red, como la detección de hosts y dispositivos activos, y la resolución de nombres de dominio.
    safe: Esta categoría incluye scripts que son considerados seguros y que no realizan actividades invasivas que puedan desencadenar una alerta de seguridad en la red.
    intrusive: Esta categoría incluye scripts más invasivos que pueden ser detectados fácilmente por un sistema de detección de intrusos o un Firewall, pero que pueden proporcionar información valiosa sobre vulnerabilidades y debilidades en la red.
    vuln: Esta categoría se enfoca específicamente en la detección de vulnerabilidades y debilidades en los sistemas y servicios que se están ejecutando en la red.

En conclusión, el uso de scripts y categorías en Nmap es una forma efectiva de automatizar tareas de reconocimiento y descubrimiento en la red. El parámetro –script permite al usuario seleccionar un conjunto de scripts personalizados para ejecutar en un objetivo de escaneo específico, mientras que las diferentes categorías disponibles en Nmap se enfocan en realizar tareas específicas para obtener información valiosa sobre la red.

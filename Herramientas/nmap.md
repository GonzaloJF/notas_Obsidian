## ¿Qué es nmap?

Nmap es la abreviatura de Network Mapper, es una herramienta de lineal de comandos de Linux, esta es de código abierto que se utiliza para escanear direcciones IP, puertos en una red y para detectar apps instaladas. 

## ¿Por qué usar Nmap?

Una de las razones para ocupar nmap es que no necesita mayores configuraciones y no es complicado de manejar por lo que tiene las características suficientes para que sea rápido y simple de ocupar .
tiene además otras características como la capacidad de reconocer rápidamente todos los dispositivos, incluido servidores, enrutadores, conmutadores, entre otros.
Ayuda a identificar los servicios que se ejecutan en un sistema, incluidos los servidores web, los servidores DNS y otras aplicaciones comunes, Detecta además versiones de las aplicaciones que es muy importante para detectar si tienen alguna vulnerabilidad. 

Algunos escaneos que se pueden ocupar:

### Escaneo de ping: 
```
> nmap -sp 192.168.1.1/24 // En este caso se ocupa para ver los dispositivos en funcionamiento de una subred determinada
```

### Escaneo de un solo Host: 
```
> namp scanme.nmap.org // En este caso se escanea los 1000 puertos más utilizados, estos puertos son los que utilizan Servicios populares como SQL,SNTP,apache,entre otros
```

### Escaneo Sigiloso: 
Algo a tener en cuenta de este tipo de escaneo es que se realiza enviando paquetes SYN y analizando la respuesta que envia de vuelta el servicio atacado, si se recibe un SYN/ACK, significa que el puerto esta abierto y se puede abrir una conexión TCP.

```
> nmap -sS scanme.nmap.org // el comando -sS es el que indica que sea silecioso el escaneo, este puede demorar más que otros tipos de escaneos ya que no es nada agresivo.
```

### Enumeración de IP de la red:
con esto podemos rastrear las maquinas de una red gracias a este comando de nmap, como restricción es que se necesita que las maquinas o dispositivos respondan a una traza ICMP:

```
> nmap -sn [rango de ip a escanear] ejemplo 192.168.1.0/24
```

Para más opciones puede revisarse: 
https://nmap.org/book/man-briefoptions.html

## Ejemplos ocupados por mí: 

```
> nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.54.117 -oG allPorts

> -p- es para que revise todos los puertos que existen.
> --open es para que reporte solamente los puertos que estan abiertos, no los que estan cerrados o que no se esta 100% seguros que esten abiertos.
> -sS el escaneo silencioso.
> --min-rate 5000 es para que no se demore más de 5000 paquetes por segundo en responder y no se alargue tanto el reconocimiento de puertos.
> -vvv es el triple verbose para que muerte en pantalla los puertos mientras escanea
> -n para que no haga el DNS resolution
> -Pn para saltar el host discovery.
> -oG es un tipo de salida del output en este caso es en formato grepable
```
cuando tenemos los puertos ya descubiertos podemos hacer lo siguiente: 
``` 
> nmap -p135,139,445,1433,5985,47001,49664,49665,49666,49667,49668,49669 -sCV 10.129.54.117 -oN targeted

> -p[n°] es para indicar que puertos quieres scanear.
> -sC es para probar los scripts más importantes y usados en los puertos abiertos.
> -sV es para encontrar los servicios y versiones que estan ocupando dichos puertos dentro de la ip
> -oN es un tipo de output que es Nmap, que te entrega la informacion más detallada. 
```

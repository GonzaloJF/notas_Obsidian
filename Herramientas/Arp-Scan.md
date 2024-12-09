arp-scan es una herramienta en lineal de comando para el descubrimiento u reconocimiento de sistemas. Construye en vía peticiones ARP  hacia direcciones IP especificas, además muestra las respuestas recibidas.

```
> arp-scan -I ens33 --localnet --ignoredups

>-I es para poner cual es la interfaz de red 
>--localnet para poder decirle que solo vea la net local 
>--ignoredups para que ignore duplicados 
```

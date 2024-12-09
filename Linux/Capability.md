En esta seccion solo veremos el ejemplo que nos recomendo savitar hacer, primero vemos que aplicaciones tienes capabilitys esto se hace con el comando: 

getcap -r / 2>/dev/null/.

para darle una capability distinta a la que ya posee o no una aplicación se hace empleando el siguiente comando: 

setcap cap_setuid+ep [directorio de la app].
  
para quitar alguna capability se hace con el comando 

setcap -r [directorio]
  
hay otros tipos de capabilitys por lo que no necesariamente te exponen a un ataque cibernético.

Para más info: 

https://book.hacktricks.xyz/v/es/linux-hardening/privilege-escalation/linux-capabilities
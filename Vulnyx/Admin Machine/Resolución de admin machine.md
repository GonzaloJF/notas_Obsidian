## Fase de reconocimiento: 
Lanzamos Arp-scan: 
![[Pasted image 20240826105019.png]]
tenemos las siguientes ips, por lo que le hacemos un ping a cada una de ellas para ver cual es la maquina a vulnerar: 
![[Pasted image 20240826105113.png]]
vemos que la 192.168.1.89 tiene un ttl de 128 por lo que podemos intuir que es la maquina windows a vulnerar. 
empezaremos haciendo los reconocimientos de puertos con nmap. 
![[Pasted image 20240826115826.png]]
tenemos los siguientes puertos abiertos. 
![[Pasted image 20240826122952.png]]
le lanzamos los scripts básicos y también para saber la información relevante de estas. 
intentamos obtener información sobre el puerto 80  que tiene una pagina con el dominio IIS windows
![[Pasted image 20240826123135.png]]
 luego de esto intentamos hacer una enumeracion del smb pero no tenemos acceso ya que no tenemos credenciales: 
 ![[Pasted image 20240826183420.png]]
 intentamos ahora con crackmapexec:  ![[Pasted image 20240826194450.png]]
 podemos ver que no hay respuestas positivas a la enumeración. 
 por lo que podemos volver al fuzzing.
 ![[Pasted image 20240826200504.png]]
 tenemos que ver las extensiones que son exclusivas de windows para poder hacer bien una búsqueda de extensiones de archivos.
 ![[Pasted image 20240826201815.png]]
 ahora que ya sabemos que se llama hope el usuario podemos hacer fuerza bruta para poder encontrar la contraseña del smb, por lo que ahora tenemos que hacer lo siguiente: 
 ocuparemos crackmapexec para encontrar la contraseña ocupando el siguiente codigo
`$ crackmapexec smb <ip> -u 'user' -p /usr/share/wordlist/rockyou.txt`
quedando algo así 
![[Pasted image 20240827125237.png]]
iremos viendo las respuestas hasta que encontremos al correcta. 
![[Pasted image 20240827125309.png]]
luego de muchos intentos podemos intentar hacer un --rid-brute
![[Pasted image 20240827131953.png]]
en este momento ya sabemos que usuarios están registrados en la maquina, la maquina por lo visto esta en español.
intentamos hacer fuerza bruta en administrador pero termina la ejecución en error
![[Pasted image 20240827194158.png]]
ahora intentaremos conectarnos a la maquina a través de evil-winrm
![[Pasted image 20240827194312.png]]
aplicamos el inicio de evil-winrm con las credenciales que tenemos
![[Pasted image 20240827195156.png]]
descargo winPeas64.exe del siguiente enlace [https://github.com/peass-ng/PEASS-ng/releases]
una vez lanzamos el reconocimiento con esta herramienta nos tenemos que fijar en esto: 
![[Pasted image 20240827205941.png]]
dentro de ese archivo podemos encontrar cosas interesantes
Aquí mismo podemos encontrar un recuento del historial de comandos
![[Pasted image 20240828144410.png]]
con esto podemos entrar como administrador 
![[Pasted image 20240828145400.png]]
y ahí encontramos las flags.
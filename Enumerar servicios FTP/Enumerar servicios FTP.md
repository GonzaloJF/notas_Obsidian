Tenemos primero a uno de los puertos más conocido que es el puerto 21, que lo ocupa comúnmente el servicio FTP, que la sigla viene de File Transfer Protocol, dentro de este se ocupa básicamente con el comando ftp:
```
$ ftp <ip>
```
algunas veces podemos intentar identificarnos como el usuario invitado, cuando hacemos el comando ftp, después de eso te piden usuario en el cual se pone anonymous, y después apretamos enter para entrar como usuario invitado.

Para aprender de FTP, haremos 2 labs que estan en github, en este caso son estos 2: 
- **Docker-FTP-Server**: [https://github.com/garethflowers/docker-ftp-server](https://github.com/garethflowers/docker-ftp-server)
- **Docker-ANON-FTP**: [https://github.com/metabrainz/docker-anon-ftp](https://github.com/metabrainz/docker-anon-ftp)

-----
## Docker-FTP-Server:

primero trabajaremos el FTP server, implementaremos lo mismo que dice dentro de github:
![[Pasted image 20240212152643.png]]
cambiamos solamente lo de FTP_USER y FTP_PASS, por gonza y peanut (contraseña que esta en el archivo de rockyou.txt), después con un `docker ps` podemos ver que esta montado la imagen del servidor ftp.
para luego intentar conectarnos al localhost, con un ``ftp localhost``, primero erramos en la contraseña, y vemos que el error es el 530, para luego tenemos que la conexión es buena por lo que podemos ver que da un codigo de estado 230. 
![[Pasted image 20240212154549.png]]
si hacemos un nmap al puerto 21 podemos ver lo siguiente:
```
$ nmap -p21 -sCV 127.0.0.1
```
![[Pasted image 20240212155135.png]]
ahora para tratar de encontrar la contraseña del usuario en cuestion podemos hacerlo con fuerza bruta, en este caso ocuparemos hydra, es una herramienta que intenta encontrar las contraseñas de distintos tipos de cuentas en distintos tipos de servicios en este caso se ejecuta el siguiente comando: 
dentro de los ejemplos que aparecen estan: 
```
  hydra -l user -P passlist.txt ftp://192.168.0.1
  hydra -L userlist.txt -p defaultpw imap://192.168.0.1/PLAIN
  hydra -C defaults.txt -6 pop3s://[2001:db8::1]:143/TLS:DIGEST-MD5
  hydra -l admin -p password ftp://[192.168.0.0/24]/
  hydra -L logins.txt -P pws.txt -M targets.txt ssh

donde las mayusculas es por que no se sabe o el usuario o la contraseña entre otros datos por lo que se les pasa un diccionario, y si los parametros estan en minusculas es por que se saben los datos que se tienen que ingresar
```
en este caso ocuparemos el primer ejemplo, ya que conocemos el usuario pero no su contraseña: 
```
$ hydra -l gonza -P /usr/share/wordlist/rockyou.txt ftp://127.0.0.1 -t 25
```
![[Pasted image 20240212161341.png]]
en este caso podemos ver que encontró la contraseña del usuario. 

----
## Docker-ANON-FTP
En este ejemplo veremos el caso en el que el usuario invitado anonymous esta activo dentro del servicio ftp, al hacer un nmap con los scripts básicos de reconocimiento más el de versiones y servicios, podemos ver que este nos detecta que esta el usuario anonymous activo: 
![[Pasted image 20240212162258.png]]
ahora si intentamos entrar con el comando ftp podemos ver lo siguiente: 
![[Pasted image 20240212162751.png]]
que entramos sin problemas con el usuario anonymous. 

--- 
## Datos de la clase: 

En esta clase, hablaremos sobre el protocolo de transferencia de archivos (**FTP**) y cómo aplicar reconocimiento sobre este para recopilar información.

FTP es un protocolo ampliamente utilizado para la **transferencia de archivos** en redes. La enumeración del servicio FTP implica recopilar información relevante, como la versión del servidor FTP, la configuración de permisos de archivos, los usuarios y las contraseñas (mediante ataques de fuerza bruta o guessing), entre otros.

A continuación, se os proporciona el enlace al primer proyecto que tocamos en esta clase:

- **Docker-FTP-Server**: [https://github.com/garethflowers/docker-ftp-server](https://github.com/garethflowers/docker-ftp-server)

Una de las herramientas que usamos en esta clase para el primer proyecto que nos descargamos es ‘**Hydra**‘. Hydra es una herramienta de pruebas de penetración de código abierto que se utiliza para realizar ataques de fuerza bruta contra sistemas y servicios protegidos por contraseña. La herramienta es altamente personalizable y admite una amplia gama de protocolos de red, como HTTP, FTP, SSH, Telnet, SMTP, entre otros.

El siguiente de los proyectos que utilizamos para desplegar el contenedor que permite la autenticación de usuarios invitados para FTP, es el proyecto ‘**docker-anon-ftp**‘ de ‘**metabrainz**‘. A continuación, se os proporciona el enlace al proyecto:

- **Docker-ANON-FTP**: [https://github.com/metabrainz/docker-anon-ftp](https://github.com/metabrainz/docker-anon-ftp)
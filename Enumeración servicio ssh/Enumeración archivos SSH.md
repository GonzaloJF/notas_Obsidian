## Apuntes de la clase: 
la imagen que ocuparemos para este ejemplo es el siguiente:
```
docker run -d \
  --name=openssh-server \
  --hostname=hack4u-academy\
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -e SUDO_ACCESS=true \         
  -e PASSWORD_ACCESS=true \     
  -e USER_PASSWORD=peanut \   
  -e USER_NAME=gonza \    
  -p 2222:2222 \ 
  -v /path/to/appdata/config:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/openssh-server:latest
```
bueno una vez descargada e instalada la imagen, podemos comenzar probando este conectándonos a través de ssh a la maquina: 
![[Pasted image 20240212164553.png]]
ahora para poder encontrar la contraseña de este con fuerza bruta ocupando hydra, podemos hacerlo de la siguiente manera: 
```
$ hydra -l gonza -P /usr/share/wordlist/rockyou.txt ssh://127.0.0.1 -s 2222 -t 15
```
![[Pasted image 20240212165112.png]]
para ver el servicio y lo demás podemos ocupar nmap al igual que en el ftp:
![[Pasted image 20240212165701.png]]
para poder ver que tipo de ubuntu o debian, o distribución de linux se esta ocupando se puede hacer una búsqueda de la versión del servicio que a veces indica la versión de Ubuntu, copiamos y pegamos esa información en Google agregando launchpad al final de la búsqueda.


----
## Datos de la clase:
En esta clase, exploraremos el protocolo **SSH** (**Secure Shell**) y cómo realizar reconocimiento para recopilar información sobre los sistemas que ejecutan este servicio.

SSH es un protocolo de administración remota que permite a los usuarios **controlar** y **modificar** sus servidores remotos a través de Internet mediante un mecanismo de **autenticación seguro**. Como una alternativa más segura al protocolo **Telnet**, que transmite información sin cifrar, SSH utiliza **técnicas criptográficas** para garantizar que todas las comunicaciones hacia y desde el servidor remoto estén cifradas.

SSH proporciona un mecanismo para autenticar un usuario remoto, transferir entradas desde el cliente al host y retransmitir la salida de vuelta al cliente. Esto es especialmente útil para administrar sistemas remotos de manera segura y eficiente, sin tener que estar físicamente presentes en el sitio.

A continuación, se os proporciona el enlace directo a la web donde copiamos todo el comando de ‘docker’ para desplegar nuestro contenedor:

- **Docker Hub OpenSSH-Server**: [https://hub.docker.com/r/linuxserver/openssh-server](https://hub.docker.com/r/linuxserver/openssh-server)

Cabe destacar que a través de la versión de SSH, también podemos identificar el codename de la distribución que se está ejecutando en el sistema.

Por ejemplo, si la versión del servidor SSH es “**OpenSSH 8.2p1 Ubuntu 4ubuntu0.5**“, podemos determinar que el sistema está ejecutando una distribución de Ubuntu. El número de versión “**4ubuntu0.5**” se refiere a la revisión específica del paquete de SSH en esa distribución de Ubuntu. A partir de esto, podemos identificar el **codename** de la distribución de Ubuntu, que en este caso sería “**Focal**” para Ubuntu 20.04.

Todas estas búsquedas las aplicamos sobre el siguiente dominio:

- **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)
## *Fase de reconocimiento:*
![[Pasted image 20240912200317.png]]

## *Fase de escaneo de puertos*
*Que puertos están abiertos*
![[Pasted image 20240912200359.png]]
*escaneo de servicios y versiones*
![[Pasted image 20240912200734.png]]
*enumeración de contenido web*
![[Pasted image 20240913131406.png]]
ahora podemos ver que hay un keepass, en este caso es un db dentro del directorio. 
![[Pasted image 20240927163546.png]]
ahora tenemos que ver este archivo con el keepass
![[Pasted image 20240927164126.png]]
tenemos el siguiente archivo
![[Pasted image 20240928202743.png]]
con john podemos obtener la contraseña:
![[Pasted image 20240928202851.png]]
la contraseña es `dreams`
![[Pasted image 20240929125647.png]]
abrimos con keepassXC y tenemos lo siguiente
![[Pasted image 20240929125733.png]]
esa es la contraseña pero el mensaje que esta en notes dice que tenemos que reemplazar las X por el numero de usuario. 
![[Pasted image 20240929144104.png]]
con crunch hacemos un diccionario cambiando las ultimas X por cifras desde el 000 al 999
![[Pasted image 20240929171605.png]]

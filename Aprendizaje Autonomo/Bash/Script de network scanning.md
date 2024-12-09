el siguiente script quiere realizar un escaneo de la red en la que estamos en este caso ocuparemos 3 tipos distintos de escaneos.
```
 #!/bin/bash
 
 echo -e "**************************************"
 echo -e "*                                    *"
 echo -e "*          Network  Scanner          *"
 echo -e "*                                    *"
 echo -e "**************************************"
 
 function ctrl_c(){
     echo -e "\n\n [+] Exit... \n\n"
     exit 1 && tput cnorm
 }
 
 tput civis
 
 #ctrl + c
 trap ctrl_c INT
 
 
 usuario=$(whoami)
 
 echo -e "[+] You are using the user: $usuario"
 
 #nombre de la interfaz acritiva
 interfaz=$(ip -br a | grep -w 'UP' | awk '$3 ~ /^[0-9]+\./ {print $1}')
 
 Ip=$(ip -br a | awk -v iface="$interfaz" '$1 == iface {print $3}')
 
 echo "my IP_address is: $Ip"
 
 echo -e "The interface is: $interfaz"
 #segmento para descubrir ips ocupadas: 
 
 segmento_red=$(ip -br a | awk -v iface="$interfaz" '$1 == iface {print $3}' | cut -d'/' -f1 | awk -F'.' '{print $1"."$2"."$3"."}')
 
 echo -e "the network segment is: $segmento_red"
 
 function helpPanel(){
     echo -e "\n [+] Uses: "
     echo -e "-h => To display the help menu."
     echo -e "-p => To be able to deploy the scan through ping."
     echo -e "-a => To be able to deploy the scan with the arp-scan application"
     echo -e "-n => To be able to deploy the scan with netdiscover"
 }
 
 #indicadores: 
 declare -i parameter_counter=0
 
function pingScanning(){
    segmento=$segmento_red
    for i in $( seq 1 254 ); do
        timeout 1 bash -c "ping -c 1 $segmento$i" &>/dev/null && echo "[+] HOST $segmento$i - ACTIVE"
    done
    wait
    tput cnorm
}

function arpScanning(){
    iface=$interfaz
    arp-scan -I $iface --localnet --ignoredups 
}

function netScanning(){
    iface=$interfaz
    netdiscover -i $iface
}

while getopts "panh" arg; do
    case $arg in
        p) let parameter_counter+=1;;
        a) let parameter_counter+=2;;
        n) let parameter_counter+=3;;
        h) helpPanel;;
    esac
done

if [ $usuario == "root" ]; then
    echo -e "you are using the user $usuario"
    if [ $parameter_counter -eq 1 ]; then
        pingScanning
    elif [ $parameter_counter -eq 2 ]; then
        arpScanning
    elif [ $parameter_counter -eq 3 ]; then
        netScanning
    else
        helpPanel
    fi
    tput cnorm
else 
	    echo -e "Try again when you are with the user ROOT"
    helpPanel
    tput cnorm
fi

```
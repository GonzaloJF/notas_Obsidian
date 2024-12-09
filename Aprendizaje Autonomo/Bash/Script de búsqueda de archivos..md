El objetivo de este script es poder hacer búsquedas tanto en todo el sistema como en directorios concretos: 
```
 #!/bin/bash
 
 function ctrl_c (){
     echo -e "\n\n [!] Exit...!! \n\n"
     exit 1
 }
 
 #ctrl + c 
 trap ctrl_c INT
 
 function helpPanel(){
     echo -e "[+] Uses: "
     echo -e "\t\n -g -> Global search of a file"
     echo -e "\t\n -d -> Search inside the directory"
     echo -e "\t\n -s -> Search for files with suid permissions"
     echo -e "\t\n -h -> Help panel"
 }
 
 function searchGlobal(){
     FindFile="$1"
     echo -e "\n\n [+] searching $FindFile in the system"
     find / -name  $FindFile 2>/dev/null
 }
 
 function searchDirectori(){
     FindFile="$1"
     echo -e "\n\n [+] searching $FindFile in the directory"
         find . -name $FindFile 2>/dev/null
 }
 
 function searchSuidPerm(){
	 echo -e "\n\n [+] Searching for files with SUID permission in the system"
     find / -perm /4000  2>/dev/null
 }
 
 function searchSgidPerm(){
     echo -e "\n\n [+] Searching for files with SGID permission in the system"
     find / -perm /2000 2>/dev/null
 }
 
 # Indicadores 
 
 declare -i parameter=0
 
 while getopts "g:d:sbh" arg; do
     case $arg in
         g) fileName=$OPTARG; let parameter+=1;;
         d) fileName=$OPTARG; let parameter+=2;;
         s) let parameter+=3;;
         b) let parameter+=4;; 
         h) helpPanel;;
     esac
done

if [ $parameter -eq 1 ]; then
    searchGlobal $fileName
elif [ $parameter -eq 2 ]; then
    searchDirectori $fileName
elif [ $parameter -eq 3 ]; then
    searchSuidPerm
elif [ $parameter -eq 4 ]; then
    searchSgidPerm
else
    helpPanel
fi

```

La idea es ir detallando paso a paso que es lo que esta haciendo este script
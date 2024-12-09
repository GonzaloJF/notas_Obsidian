En cuanto a la Criptografia podemos llegar a necesitar saber como rotar las posiciones de las letras por lo que aquí mostrare un ejemplo de como hacer este tipo de rotaciones.
En este caso ocuparemos de ejemplo una rotacion de 13 letras es decir que si ponemos una A tiene que ser la letra que esta 13 posiciones después en el abecedario en este caso sería la N esto lo haremos con el comando tr en linux. 
```
$ tr "A-Za-z" "N-ZA-Mn-za-m"
```
ha este tipo de cifrado se le llama cifrados cesar y hay algunos que también ocupan caracteres especiales y números dentro de todas las rotaciones dentro de las más ocupadas están las rot47.

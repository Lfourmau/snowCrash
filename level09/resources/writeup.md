## Level09

Même départ que le level08, un elf “level09” et un fichier “token”.

Je tente de lancer l’exec et il demande 1 argument. Je lance donc “./level09 hey” et le programme semble me retourner ma string encodée.

Lorsque je fais un **cat token,** j’accède à une string incomprehénsible (encodée aussi surement)

Le programme **level09** semble juste incrémenter les lettres, et de plus en plus au fur et à mesure qu’on avance dans le mot. La première lettre +0, la seconde +1, la troisième +2…. 

Ca semble ne pas se limiter aux lettres mais à la table ascii de manière générale.

Peut être qu’il faut passer le contenu de token dans le programme level09 pour que token soit lisible.

J’ai essayé **cat token | ./level09** mais le programme check le nombre d’arguments donc il ne veut pas.

On peut aussi supposer du coup que le token qu’on a en faisant cat token est passé dans level09 et qu’il faut créer un programme qui fait l’inverse.

Je fais un script  pour faire un petit test

ca a l’air de bien reverse la chaine. Je vais essayer d’ajouter la lecture d’un fichier.

Le programme renvoie une chaine qui n’est pas la bonne.

![Untitled](./resources/screenshots/Untitled%209.png)

Je me dit qu’il faut peut être ignorer les chars qui sortent de la table ascii, donc j’ajoute une condition et j’obtient une string acceptable (f3iji1ju5uea411afi) mais c’est pas ca.

Je repasse cette string à level09 pour voir si ca fait le meme contenu que token et pas du tout

Mon script pour le moment c’est 

```bash
#!/bin/bash

file="$1"
content=$(cat "$file")

decrement_characters() {
    input="$1"
    length=${#input}
    result=""
    for ((i = 0; i < length; i++)); do
        char="${input:i:1}"
        ascii_code=$(printf '%d' "'$char")
        new_ascii_code=$((ascii_code - i))
        
        # Check if the new ASCII code is within the ASCII range (0-127)
        if [ "$new_ascii_code" -ge 0 ] && [ "$new_ascii_code" -le 127 ]; then
            new_char=$(printf '\\x%x' "$new_ascii_code")
            result="$result$new_char"
        fi
    done
    echo "$result"
}

result=$(decrement_characters "$content")
echo "$result"
```

Je viens de voir qu’il y a python sur la machine, ca va être + simple.

J’ai fait ce script : 

```python
# -*- coding: utf-8 -*-

import sys

def decode(input_str):
    result = ""
    for i, char in enumerate(input_str):
        ascii_code = ord(char)
        new_ascii_code = ascii_code - i

        if 0 <= new_ascii_code <= 127:
            result += chr(new_ascii_code)
    return result

file_name = sys.argv[1]

try:
    with open(file_name, 'r') as file:
        content = file.read()
        result = decode(content)
        print result
except IOError:
    sys.exit(1)
```

[https://www.commentcoder.com/python-enumerate/](https://www.commentcoder.com/python-enumerate/)

[https://www.programiz.com/python-programming/methods/built-in/ord](https://www.programiz.com/python-programming/methods/built-in/ord)

Puis j’ai fait **python [monscript] token**  et j’ai obtenu la string **f3iji1ju5yuevaus41q1afiuq** qui m’a permi de me co a flag09

J’ai lancé getflag et le flag pour se co au lvl 10 est **s5cAJpM8ev6XHw998pRWG728z**
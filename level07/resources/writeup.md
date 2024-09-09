Je fais ls et on voit un executable qui s’appelle level07. Quand on fait un file dessus on remarque qu’il s’agit d’un elf. Quand on le lance, il affiche juste level07.

Je le récupère donc sur ma machine avec **scp -P 4242** [level07@172.20.10.2](mailto:level06@172.20.10.2)**:/home/user/level07/level07 .**

Je l’ai balancé sur DogBolt et il semblerait que le main utilise la var d’environnement LOGNAME et echo :

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/16ad7c51-07e0-4d87-aa46-423fe70c1477/871dbbae-b01d-4fb9-9b82-21aaf36d8267/Untitled.png)

J’ai fait un export LOGNAME=test puis j’ai relancé le programme, il affiche bien test. 

Le programme utilise **asrpintf** pour mettre dans une variable la string “**/bin/echo [LOGNAME]**” , puis execute la commande de cette string avec system.

Je cherche donc s’il est possible d’exécuter deux commandes avec un seul system(), et la réponse est ici :https://stackoverflow.com/questions/245600/using-a-single-system-call-to-execute-multiple-commands-in-c

C’est possible avec && entre les commandes.

Je fais donc un **export LOGNAME=”&& /bin/getflag”** pour qu’après avoir fait son echo, le programme lance getflag.

Je relance level07 et voilà le flag pour se connecter au level08 : **fiumuikeil55xe9cu4dood66h**
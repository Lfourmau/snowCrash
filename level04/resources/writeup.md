## Level04
    
Le script perl contient ca :

![Untitled](./screenshots/Untitled%205.png)

On voit qu’echo est encore utilisé. Je vais tenter la même manip.

Ca ne fonctionne pas, getflag n’est pas lancé en root.

En faisant un ls -la sur le script perl on remarque que le owner est flag04.

On sait que le script perl est un serv web qui tourne sur le port 4747. Quand on va sur l’ip de la vm dans un navigateur:4747 ca ne donne rien. En revanche on voit dans le code que le script attend un paramètre appelé **X**. On tente donc un curl depuis la VM vers [localhost](http://localhost):4747 avec **curl localhost:4747?x='\`/bin/getflag\`'** et on obtient le flag.

le flag est **ne2searoevaevoem4ov4ar8ap** pour se connecter au level05

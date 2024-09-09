## Level03

Je vois dans le home un exécutable nommé level03.

Je fais un **scp -P 4242 [level03@172.20.10.2](mailto:level03@172.20.10.2):/home/user/level03/level03 .** Pour récupérer l’exécutable sur la machine hôte.

Je vais tenter de voir ce que donne le fichier sur DogBolt

On voit que le programme fait appel à /usr/bin/env puis echo:

![Untitled](./screenshots/Untitled%204.png)

Le programme utilise echo du coup on fait un script qu’on appelle echo dans /run/shm (car cet endroit est accessible en écriture). Le contenu du script est : ***exec /bin/getflag***

On chmod+x ce script, on remplace la variable d’environnement PATH avec **export PATH=/run/shm** pour que le level03 aille chercher notre echo, puis on lance level03 et tadam.

Le flag est **qi0maab88jeaj46qoumi7maus** pour se co au level 04
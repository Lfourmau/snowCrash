 ## Level08

Encore une fois, un exécutable elf est dispo dans le home. Cette fois ci on a aussi un fichier appelé token. Nous n’avons pas les droits pour lire token. Il semblerait que le flag soit dedans.

Quand on lance l’exécutable (qui s’appelle **level08**), on voit qu’il attend un fichier à lire en argument. Bien sur on essaye **./level08 token** et ca ne marche pas.

Comme d’hab, je récupère l’exécutable sur ma machine avec **scp -P 4242** [level08@172.20.10.2](mailto:level06@172.20.10.2)**:/home/user/level08/level08 .**

Après un coup sur DogBolt, on voit qu’il y a cette condition dans le code :

```bash
if (strstr(argv[1], "token") != 0)
{
printf("You may not access '%s'\n", argv[1]);
exit(1);
/* no return */
}
```

Donc le code fait juste un check sur le nom du fichier pour savoir si on essaye d’accéder à token. Peut être qu’il lien symbolique peut nous sortir de là. 

Je crée un symlink entre token et un fichier dans tmp. Quand je passe mon fichier en arg du programme j’ai un permission denied. Ca vient surement du fait qu’un fichier est détenu par flag08 et pas l’autre.

Je cherche donc les fichiers possédés par flag08, peut être que si je peux lire l’un d’eux le symlink fonctionnera. Les seuls fichiers appartenant à flag08 sont l’exécutable et token

en regardant le programme de plus près avec cutter, j’ai vraiment l’impression qu’il lit tous les fichiers sauf s’ils portent le nom token..

J’avais tenté le symlink avec un fichier dans /run/shm, je rentente mais dans /tmp avec :

**ln -s /home/user/level08/token /tmp/testfile**

puis je lance donc **./level08 /tmp/testfile**

Ca m’affiche : **quif5eloekouj29ke0vouxean** qui est le token pour se connecter à flag08. Je lance donc getflag et obtient le mdp pour le level09 : **25749xKZ8L7DkSCwJkT9dyv6f**
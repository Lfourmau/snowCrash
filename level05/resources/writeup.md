## level05
    
Pendant la reco au lvl 00 j’avais vu :

![Untitled](./screenshots/Untitled%206.png)

Du coup j’essaye de cat le fichier :

```jsx
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

Je vais dans **/usr/sbin** et je tape **ls -la | grep openarenaserver** , ce qui  démontre que openarenaserver est own par flag05. Si on arrive a faire en sorte que openarenaserver lance getflag, ce sera fait sous flag05 et on aura le flag.

Voici le contenu de openarenaserver (juste cat le fichier)

```bash
#!/bin/sh

for i in /opt/openarenaserver/* ; do
(ulimit -t 5; bash -x "$i")
rm -f "$i"                                                                                                   
done
```

On remarque alors que le programme mentionne /opt/openarenaserver, qui est un directory vide. Cependant on voit que le programme itère sur le folder.

On dirait que le programme tente d’exécuter tout ce qui se trouve dans /opt/openarenaserver

donc je met un script qui appelle getflag dedans (j’ai repris le echo des lvl précédents)

Vu la crontab, on devrait avoir le flag au bout de deux minutes si ca marche.

Je vois que le fichier que j’ai ajouté dans /opt/openarenaserver a été delete, donc la crontab est passée. J’ai pas eu d’output donc je modifie le script et je tape **echo 'getflag > /tmp/kekchose.txt' > exploit**

j’attends un peu et je vois que mon programme a été de nouveau supprimé donc je vais voir si /tmp/kekchose.txt a été créé. Je tente donc un cat et j’obtiens le flag pour se co au level 06 : 

**viuaaale9huek52boumoomioc**

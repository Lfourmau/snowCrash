## Level02 :
    
Si on fait ls on voit juste un fichier pcap. On va donc le ramener sur la machine perso pour essayer de le lire : ***scp -P 4242 [level02@192.168.1.19](mailto:level02@192.168.1.19):~/level02.pcap .***
    
On tape le mdp et on obtient le fichier.

J’essaie **tcpdump -qns 0 -A -r level02.pcap** (utilisable sans gui sur linux) et je vois marqué *password* dans un des paquets mais la valeur n’est pas affichée. Je vais essayer de le lire avec wireshark.

(j’ai du changer les permissions pour sortir le pcap du wsl.) 

Sur wireshark je vois la même chose, c’est écrit password mais je vois pas ce que c’est. Si je fais clic droit sur le paquet en question puis “suivre le flux tcp”, je vois un peu + d’infos :

![Untitled](./screenshots/Untitled%202.png)

j’ai essayé de prendre la string rouge et de la décoder sur crackstation et dcode ca n’a pas donné le bon mdp.

J’ai essayé d’envoyer le pcap sur cloudshark comme dit dans la vidéo de présentation du projet mais même résultat que WireShark.

Si on met tous les paquets sur Dcode il y a une bonne correspondance pour l’algortihme de “code Wabun” mais la version décodée ne donne rien (FTMUWANDRHEHEHENDRELHEL0L)

En fait quand on regarde les paquets avec wireshark on remarque que c’est un échange entre deux machines. L’une semble voir se connecter à l’autre en envoyants des creds mais l’autre renvoie login incorrect.

![Untitled](./screenshots/Untitled%203.png)

Sur ce screen on voit bien l’échange (rouge vs bleu), on dirait que le mot de passe est envoyé en plusieurs parties ?

Il y a pas mal de chances pour que le mdp soit juste la string de réponse (en rouge) mais sans les . : **ft_wandrNDRelL0L** mais ca ne fonctionne pas pour se co à flag02

En fait les points qu’on voit au milieu de la string rouge semblent être des “delete” (7f dans la table ascii c’est del), donc si il y a trois points par exemple, les trois derniers chars ont été effacés. SI on suit cette logique, on remarque que la personne a du mal avec les majuscules. Le mdp est donc **ft_waNDReL0L** pour se co a flag02.

Le flag est **kooda2puivaav1idi4f57q8iq** pour se co au level 03
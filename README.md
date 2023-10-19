# TP2
1 Secure Shell : SSH
1.1 Exercice : Connection ssh root (reprise fin tp-01)
- Connectez vous en root via la console `a votre machine et configurez ssh. Utilisez pour cela les
commandes apt search et apt install. Changez la configuration de ssh pour permettre les
connection root distante avec mot de pass.
- Expliquez à l’aide du man sshd_config l' élement de la configuration ssh que vous avez du
changer les diff´erentes options possibles, les avantages et leurs inconvénients. Dans quel cas faut
t’il utiliser chacunes des options.

1.2 Exercice : Authentification par clef / Génération de clefs
Vous allez maintenant cr´eer une clef d’authentification pour vous connecter directement à votre
serveur Linux. Vous devez commencer par (source: https://www.it-connect.fr/chapitres/authentification-ssh-par-cles/)
- gérérer un couple de clef privée public sur votre session Linux sur la machine hote:
- Entrer la commande ssh-keygen
- rentrer le noms du fichier dans lequel on veut enregistrer les clés (j'ai choisi le fichier .ssh/id/rsa)
- laisser le passphrase vide 2 fois on ne mets pas de passphrase car:
  -  on doit entrer la passphrase chaque fois qu'on veut utilisez la clé privée
  -  aussi, si on oublie la passphrase, on ne peut plus accéder à notre clé privée..
- le couple de clé est généré et mis dans le fichier sélectionner

1.3 Exercice : Authentification par clef / Connection serveur
-Configuration sur le serveur

Il faut indiquer à ssh que nous souhaitons permettre la connexion par clés pour cela, aller dans le fichier /etc/ssh/sshd_config et faire PermitRootLogin yes
Après cette étape, je n'ai pas eu à faire la création ou moditication du dossier .ssh ça s'est fait tout seul après la génération de mon couple de clé privée/publique

1.4 Exercice : Authentification par clef depuis la machine hote :

pour se connecter à la machine haote, il faut rentrer la commande:ssh -i maclef.pub root@ipserveur et la connection sera établi.

1.5 Exercice : S´ecurisez

Pour sécurisez l’accès à notre machine via ssh pour root par clef seulement afin d’´eviter les tentatitve d’authentification par brute force ssh il faut:

-générer un couple de clé privée et publique (déjà fait avant)
-Copier la clé publique sur le serveur de la machine hote avec la commande : ssh-copy-id user@hostname
-désactiver l'authentification par mot de passe dans le fichier de configuration SSH (/etc/ssh/sshd_config) faire : PasswordAuthentication no
-redémarrez le service SSH avec: systemctl restart ssh

Expliquez en quoi consiste les attaques de type brute-force ssh: les attaques de type brute-force SSH consistent à essayer de deviner un mot de passe en essayant de nombreuses combinaisons possibles

D’autres techniques permettant à l’administrateur du serveur de ce protéger de ce typed’attaques:
1. Utilisation de comptes d'utilisateurs distincts : Au lieu d'utiliser le compte "root" pour l'administration il faut plutot créer des comptes d'utilisateurs distincts avec des privilèges limités.
Avantage : une réduction des risques en cas de compromission d'un compte utilisateur.
Inconvénient : plus de configuration à gérer, nécessite une utilisation correcte de "sudo".
2. Mises à jour régulières : Assurez-vous que le système d'exploitation et les logiciels installés sont toujours à jour avec les dernières correctifs de sécurité.
Avantage : diminue les vulnérabilités connues.
Inconvénient : il faut une maintenance régulière.

2 Processus
2.1 Exercice : Etude des processus UNIX
1 - En utilisant la commande ps on affiche la liste de tous les processus tournant sur votre machine: ps -eo user,pid,%cpu,%mem,stat,start,time,command | head -n5  
-A quoi correspond l’information TIME ? ça correspond à la durée totale du temps CPU utilisée par le processus depuis son démarrage.
-Quel est le processus ayant le plus utilisé le processeur sur votre machine ? en utilisant la commande top, le processus le plus utilisé est Top
-Quel a été le premier processus lancé aprè le démarrage du système ? en utilisant la commande ps -p 1, le processus lancé est systemd
-A quelle heure votre machine a-t-elle démarrée ? Pour savoir à quelle heure la machine a été démarrée, on utiliser la commande: uptime (l'heure c'est 15h43min49s)
La commande uptime permet également de trouver le temps depuis lequel votre serveur tourne et moi c'était 1h32.
-Pouvez-vous établir le nombre approximatif de processus créés depuis le d´emarrage (“boot”) devotre machine ? 
Il faut utiliser la commande : ps -eo pid,etimes,comm --sort=start_time | grep -v "PID" | awk '$2 > 0' | wc -l

2 - Sous UNIX, chaque processus (except´e le premier) est cr´e´e par un autre processus, son
processus p`ere. Le processus p`ere d’un processus est identifi´e par son PPID (Parent PID).
– Une option de la commande ps permettant d’afficher le PPID d’un processus: ps -o ppid= -p <PID> (remplacer PID par le PID du processus ciblé)
– Pour donner la liste ordonnée de tous les processus ancetres de la commande ps en cours d’exécution on utilise la commande : pstree -p <PID>

3 - Reprendre la question précédente avec la commande pstree.
Pour installer le package pstree, il faut:
- entrer apt install pstree
- faire apt install psmisc

4 - Essayez la commande top, qui affiche les memes informations que ps mais en raffraichissant
périodiquement l’affichage.
- La touche ? permet d’afficher un résumé de l’aide de top. Afficher dans top la liste de processus
triée par occupation mémoire (“resident memory”) décroissante il faut faire Shift + M
- Quel est le processus le plus gournabnd sur votre machine ? Top
- A quoi correspond-il ? Affichage en temps réel des informations sur l'utilisation du processeur et de la mémoire, ainsi que la liste des processus en cours d'exécution.
- Trouvez les commandes interactives permettant de :
  *passer l’affichage en couleur : Shift + Z
  *mettre en avant le colonne de trie (la touche "M" (majuscule) pour effectuer le tri par utilisation de mémoire de manière décroissante et 
  *changer la colonne de trie.
- Essayez la commande htop. Expliquez les avantages et/ou inconvénients à son utilisation par rapport à top.
  *Avantages:
  -Contrôle interactif : on peut utiliser les touches fléchées pour naviguer dans la liste des processus et effectuer des actions
  -Interface plus conviviale : htop offre une interface utilisateur plus conviviale et plus informative que top. Il présente les informations de manière plus structurée et propose une vue plus agréable.
  *Inconvénients:
  -Nécéssite une installation: htop n'est pas inclus dans l'installation de base des systèmes d'exploitation, tandis que top est plus couramment préinstallé.
  -Accessibilité : Pour utiliser htop, vous devrez peut-être l'installer manuellement, ce qui peut nécessiter des privilèges d'administration.

3 Exercice 2 : Arret d’un processus
Ecrivez deux script shell contenant des boucles affichant la date.
-fichier date.sh
#!/bin/sh
while true; do sleep 1; echo -n ’date ’; date +%T; done
Pour se faire , il faut :
  -utiliser un éditeur (nano ou vi) en faisant vi/nano date.sh
  -copier coller le script 
  -enregistrer en appuyant sur la touche Esc pour quitter le mode d'édition, puis taper :w et appuyez sur Enter
-fichier date-toto.sh
#!/bin/sh
while true; do sleep 1; echo -n ’toto ’; date --date ’5 hour ago’ +%T; done
  -meme procédure que pour le fichier date.sh
- Lancer le 1er scripts en faisant bash date.sh. Le mettre en arrière plan (CTRL-Z).
- Lancer le 2eme scripts en faisant bash date-toto.sh. Le mettre en arrière plan (CTRL-Z).
- La commande:
  -jobs :montre les processus stoppés
  -fg :montre relance le processus
  -CTRL-C :arreter les 2 horloges.
- La commande kill (kill - 9 "PID") stoppe brutalement le processus

4 Exercice 3 : les tubes
Quelle est la différence entre tee et cat ?
  -La commande cat est principalement utilisée pour afficher le contenu d'un ou plusieurs fichiers à l'écran ou pour concaténer (fusionner) le contenu de plusieurs fichiers en un seul fichier de sortie.
  Elle est souvent utilisée pour lire le contenu d'un fichier dans la sortie standard ou pour fusionner le contenu de plusieurs fichiers en un seul fichier.
  -La commande tee permet de lire l'entrée standard et d'écrire simultanément ce contenu dans un fichier et de l'afficher à l'écran. En d'autres termes, tee divise le flux de données en deux directions : une vers un fichier et l'autre vers la sortie standard.
Que font les commandes suivantes :
$ ls | cat : liste les fichiers
$ ls -l | cat > liste : prend le résulat de la requete "ls -l" et la met dans le fichier liste
$ ls -l | tee liste :  le résultat de la requete "ls -l" sera affiché à l'écran, et en même temps, ce résultat sera enregistré dans un fichier appelé "liste" dans le répertoire actuel
$ ls -l | tee liste | wc -l : affiche toutes les ligne du résultat à l'écran de "ls -l | tee liste"

5 Journal syst`eme rsyslog
- A quoi sert le service cron ? C'est est un planificateur de tâches dans les systèmes d'exploitation de type Unix et, il est utilisé pour automatiser l'exécution de tâches à des moments précis et de manière récurrente, que ce soit quotidiennement, hebdomadairement, mensuellement ou à d'autres intervalles
- Que fait la commande tail -f ? affiche le contenu d'un fichier (tail -f nom_du_fichier)
- A l’aide de cette commande, placer en bas de votre écran une fenétre qui permette de visualiser en “temps réel” le contenu du fichier /var/log/messages.
Que voyez-vous si vous redémarrez le service cron depuis un autre shell ? on voit un exemple de planification de tache possible 
- Expliquer à quoi sert le fichier /etc/logrotate.conf. C'est un fichier de configuration utilisé par le système de gestion des journaux sous Linux. Il définit les paramètres globaux et les règles pour la rotation et l'archivage des fichiers journaux sur le système
- Examiner la sortie de la commande dmesg.
  - Quel modèle de processeur linux détecte-il sur votre machine ? en faisant la commande lscpu on obtiens le nom du processeur: 12th Gen INTEL(R) Core(TM) i7-12700
  - Quels modèles de cartes réseaux détecte-il ? en faisant la commande dmesg | grep -i 'eth' on a eth0: INTEL(R) PRO/1000 Network Connection

 Remarque Générale: ce TP était beaucoup plus facile que l'aute j'ai eu quelques petits soucis ne sachant pas qu'il fallait installer certains outils avant l'utilisation comme pstree et htop par exemple. Sinon C'était beaucoup plus facile et rapide à faire .
 Source: Prof, Chatgpt, man, www.malekal.com, www-itconnect.com, cloud.ibm.com, perso.liris.cnrs.fr et www.digitalocean.com





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
Configuration sur le serveur
Il faut indiquer à ssh que nous souhaitons permettre la connexion par clés pour cela, aller dans le fichier /etc/ssh/sshd_config et faire PermitRootLogin yes
Après cette étape, je n'ai pas eu à faire la création ou moditication du dossier .ssh ça s'est fait tout seul après la génération de mon couple de clé privée/publique


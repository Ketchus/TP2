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

1.4 Exercice : Authentification par clef : depuis la machine hote
pour se connecter à la machine haote, il faut rentrer la commande:ssh -i maclef.pub root@ipserveur et la connection sera établi.

1.5 Exercice : S´ecurisez
Pour sécurisez l’accès à notre machine via ssh pour root par clef seulement afin d’´eviter les tentatitve
d’authentification par brute force ssh il faut:
-générer un couple de clé privée et publique (déjà fait avant)
-Copier la clé publique sur le serveur de la machine hote avec la commande : ssh-copy-id user@hostname
-désactiver l'authentification par mot de passe dans le fichier de configuration SSH (/etc/ssh/sshd_config) faire : PasswordAuthentication no
-redémarrez le service SSH avec: systemctl restart ssh

Expliquez en quoi consiste les attaques de type brute-force ssh: les attaques de type brute-force SSH consistent à essayer de deviner un mot de passe en essayant de nombreuses combinaisons possibles

Trouvez et expliquez d’autres techniques permettant `a l’administrateur du serveur de ce prot´eger de ce type
d’attaques. Note il existes plusieurs m´ethodes avec avantages et conv´enients (expliquez en
fonction de vos choix).
1- Utilisation de comptes d'utilisateurs distincts : Au lieu d'utiliser le compte "root" pour l'administration il faut plutot créer des comptes d'utilisateurs distincts avec des privilèges limités.
Avantage : une réduction des risques en cas de compromission d'un compte utilisateur.
Inconvénient : plus de configuration à gérer, nécessite une utilisation correcte de "sudo".
2-Mises à jour régulières : Assurez-vous que le système d'exploitation et les logiciels installés sont toujours à jour avec les dernières correctifs de sécurité.
Avantage : diminue les vulnérabilités connues.
Inconvénient : il faut une maintenance régulière.

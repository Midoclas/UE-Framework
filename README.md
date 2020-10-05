# Support module L3
La première étape a éffectué consiste à installer votre environnement de développement.
Il y a plusieurs façon de le faire, les critères principaux à retenir sont :
- Isolation les actions et développement que vous faites ne doivent pas perturber (poluer) le reste de votre système
- Evolution/Regression, vous devez pouvoir changer de version de librairie de langage en gardant la même architecture.
- Partage/Colaboration, un projet important ne se développement jamais seul et sera repris par d'autre, 
le temps que vous avez passé à configurer ne doit pas être perdu, il faut capitaliser
- Sauvegarde, on est jamais à l'abris d'une panne, votre environnement doit pourvoir se redéployer à partir d'un backup
- Historisation, les choix que vous faites à un moment dans vos développements peut être remis en cause, 
versionnez votre code pour pouvoir revenir en arrière 
### Environnement de Dev sous docker (conseillé)
Le Dockerfile fournit permet de construire une image prête pour installer le framework Symfony. Ensuite lancer une version
exécutée de cette image : le container

    $ docker build -t php/symfony .
    $ docker run --rm -ti --name symfony -u $UID:$GROUPS -p 80:8000 -v `pwd`/project-tmp:/opt/project php/symfony /bin/bash
    
Sous *nix l'image est exécutée par defaut par l'user root, ajouter l'option -u $UID:$GROUPS pour que les fichiers créés
par le container vous appartienne. Sous Windows remplacer `pwd` par C:\votre_repertoire_de_projet 

    $ docker run --rm -ti --name symfony -p 80:8000 -v C:\votre_repertoire_de_projet:/opt/project php/symfony /bin/bash

Cette première étape passée, il faudra lancer le script de création d'un projet pour une application web.

    $ docker exec symfony composer create-project symfony/website-skeleton .

Puis démarrer le serveur Web (Attention c'est dans le container symfony l'exemple ci-dessous)

    $ symfony symfony server:start  

Ouvrez votre navigateur sur l'URL http://localhost pour avoir la page d'accueil de symfony

### Environnement de Dev avec une VM
Vous pouvez travaillé en exécutant vos développements dans une VM. Les explications suivantes se font à partir d'une 
VM sous linux et ont été testées avec "Ubuntu Server 18.04".  
La première étape consiste à installer l'ensemble des paquets pour PHP, c'est à dire : PHP, composer, zip, PHP-xml. Composer,
 le gestionnaire de paquets PHP, permet l'installation de Symfony. PHP-xml est un module PHP utilisé par Symfony.

    $ sudo apt update
    $ sudo apt install php
    $ sudo apt install composer
    $ sudo apt-add-repository multiverse
    $ sudo apt install composer
    $ sudo apt install zip
    $ sudo apt install php-xml

Pour que vos développements soient présents dans le système de fichier de votre VM et puissent être excécutés, vous avez deux
possibilités : lier les deux systèmes de fichier (via le partage de VirtualBox par exemple ou l'upload sur la VM comme pour
une machine en production. Dans la deuxième possibilité est peut-être plus facile et surtout vous allez devoir la mettre en
place dans votre projet pour l'upload sur le serveur.  
L'upload de fichier sur un serveur (votre VM par exemple) demande la mise en place d'un système de transfert de fichier 
(ftp, sftp ... ) pour la liaison de deux systèmes de fichier ça sera plustôt (nfs, samba ... ), ci-dessous la commande
pour l'installation du service sshd pour permettre l'accès en ssh et en sftp sur votre VM. (Remarque si vous avez installé
la distribution ubuntu server ce service sera déjà installé)

    $ sudo apt install openssh-server
    $ sudo systemctl start ssh
    
La deuxième ligne sert à lancer le service (si il tourne déjà ça ne fera rien). Vous pouvez maintenant utiliser votre IDE
pour uploader/downloader vos fichiers.
La partie serveur de dev étant fini, passez maintenant à la mise en place de votre projet Symfony, comme ci-dessus.
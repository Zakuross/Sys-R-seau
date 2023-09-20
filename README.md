# Sys-R-seau

L'authentification peut se faire sans l'utilisation de mot de passe ou de phrase secrète en utilisant la cryptographie asymétrique.
La cryptographie asymétrique (ou cryptographie à clé publique) permet d'assurer la confidentialité d'une communication ou d'authentifier les participants,
sans que cela repose sur une donnée secrète partagée entre ceux-ci, au contraire d'une cryptographie symétrique qui nécessite ce secret partagé au préalable.

La clé publique est distribuée sur les systèmes auxquels on souhaite se connecter.
La clé privée, qu'on protège par un mot de passe, reste uniquement sur le poste à partir duquel on se connecte.
L'utilisation d'un "agent ssh" permet de stocker le mot de passe de la clé privée pendant la durée de la session utilisateur.

Cette configuration profite aussi à SCP et SFTP qui se connectent au même serveur SSH.

Pincipe de fonctionnement des clés SSH :

tuto ubuntu
Liste des commandes terminal

    Navigation et Gestion de Fichiers :
        ls = listing : liste le contenu du répertoire courant par ordre alphabétique.
        cd = change directory : permet de naviguer d'un répertoire à un autre.
        pwd = print working directory : permet d'afficher le chemin d'accès vers le répertoire ou se situe l'utilisateur qui a entré la commande.
    Manipulation de fichiers et de répertoires :
        cp = copie : permet de copier un fichier ou un groupe de fichiers ou de répertoires.
        mkdir = make directory : permet de créer un nouveau répertoire (dossier).
        touch = permet de créer un nouveau fichier. Il doit être suivi du nom du fichier et de son extension.
        mv = move : permet de déplacer un fichier ou un dossier d'une source vers une autre destination.
    Affichage et lecture de contenu de fichier :
        cat = concatenate : permet de créer, fusionnet ou imprimer des fichiers dans l'écran de résultat standard ou vers un autre fichier.
        less = permet de visualiser un fichier texte page par page (sans le modifier).
        find = permet de chercher des fichiers dans un ou plusieurs répertoires selon des critères définis par l'utilisateur
        grep = global regular expression print : permet de recherche une chaîne de caractères dans un fichier spécifié.
    Transfert et Synchronisation de fichiers :
        scp = secure copy : permet de copier en toute sécurité des fichiers depuis notre ordinateur local vers des serveurs distants, et inversement, à l'aide du protocole SSH.
        rsync = permet de synchroniser des fichiers localement et à distance. On peut ainsi transférer des fichiers et des répertoires.
    Editeur de Texte :
        vim = c'est un éditeur de texte en ligne de commande
        nano = autre éditeur de texte en ligne de commande
    vérifier les ports :
        lsof = commande basique native de Linux pour connaître les ports ouverts dans le system.
        cette commande montre plusieurs informations : nom de l'application (ex: sshd), le douille du programme (adresse IP associée au port) et l'identifiant du processus (PID).
        sudo lsof -i -P -n ou sudo lsof -i -P -n | grep LISTEN

Mise en production d'applications sans conteneurisation
PHP-FPM et NGINX

PHP-FPM :
il s'agit de l'accronyme pour FastCGI Process Manager (= gestionnaire de processus FastCGI).
C'est une interface SAPI permettant la communication entre un serveur web et PHP, basée sur le protocole FastCGI.

FastCGI est une technique permettant la communication entre un serveur web et un logiciel indépendant, 
c'est une évolution de Common Gateway Interface (CGI) = Interface passerelle commune.

SAPI (Server Application Programming Interface = interface de programmation des applications serveurs)
est le terme générique utilisé en informatique pour désigner les modules d'interface d'applications serveur web comme Apache,
Internet Information Services ou iPlanet.

FPM est une alternative à l'implémentation PHP FastCGI avec des fonctionnalités supplémentaires utiles pour les sites fortement chargés.
Fonctionnalités inclues :

    Gestion avancée des processus avec stop/start doux (graceful) ;
    Pools qui donnent la possibilité de démarrer des travailleurs avec différents uid/gid/chroot/environnement,
    écoutant sur différents ports et utilisant différents php.ini (remplace le safe_mode) ;
    Configurable journalisation stdout et stderr ;
    Redémarrage d'urgence en cas de destruction accidentelle du cache opcode ;
    Support de l'upload acccéléré ;
    "slowlog" - journalisation des scripts (pas juste leurs noms, mais leur backtrace PHP également, utilisant ptrace ou équivalent pour lire le processus distant)
    qui s'éxecutent de manière anormalement lente ;
    fastcgi_finish_request() - fonction spéciale pour terminer la requête et vider toutes les données tout en continuant d'exécuter une tâche consommatrice
    (conversion vidéo par exemple) ;
    Naissance de processus fils dynamic/ondemand/static ;
    Informations d'état de base et étendues (similaire à mod_status d'Apache) avec différents formats supportés comme json, xml et openmetrics ;
    Fichier de configuration basé sur php.ini ;

NGINX :
Nginx est un logiciel de serveur distribué en 2004.
L'objectif du programmeur derrière nginx (Sysoev) était de créer un serveur de haute performance qui peut gérer plusieurs clients web en même temps.

il s'agit d'un système asynchrone qui utilise les changements d'état pour gérer plusieurs connexions en même temps.
Le traitement de chaque requête est découpé en de nombreuses mini-tâches et permet ainsi de réaliser un multiplexage efficace entre connexions.
Afin de tirer parti des ordinateurs multiprocesseurs, plusieurs processus peuvent être démarrés.
Ce choix d'architecture entraîne des performances très élevées, ainsi qu'une charge et une consommation de mémoire très inférieures à celles des serveurs HTTP classiques comme Apache.

Un serveur Nginx permet d'utiliser PHP-FPM pour traiter les scripts PHP.
La combinaison NGINX/PHP-FPM permet de servir ses applications PHP en production.
![image](https://github.com/Zakuross/Sys-R-seau/assets/131243116/fdeaecbb-5814-4c7f-8397-84fa1644db10)
![image](https://github.com/Zakuross/Sys-R-seau/assets/131243116/3b859718-09c6-4ba7-8aad-c99677bb44ee)
Mise en production d'applications avec conteneurisation
Introduction de Docker et conteneurisation moderne

Historique

De manière générale, presque toutes les entreprises utilisent l'environnement cloud (privé ou public) avec des instances exécutant des Virtual Machines
avec des capacité d'évolutivité et d'équilibrage de charge représentant leur couche de calcul.
Avec l'évolution des défis, ce genre d'environnements sont devenus inefficaces :

    Manque de cohérence des environnements : déploiement d'applications et de packages dans des environnements virtuels.
    Dépendance du système d'exploitation : les applications déployées sont exécutées uniquement sur des systèmes d'exploitation compatibles.
    Niveau d'isolement : incapacité à fournir un sandbox instantané au dessus du niveau du système d'exploitation.
    Granularité de la consommation de calcul : impossibilité de déployer plusieurs applications répliquées, alors que l'équilibrage de la charge sur la couche applicative ne se produit qu'au sein d'une seule machine et non au niveau de la couche du système d'exploitation.
    Correctifs des images dans les environnements de production : Les déploiements canari et bleu-vert ne sont pas flexibles au niveau du cluster et son difficiles à gérer dans plusieurs régions.

Pour répondre à ces problèmes, on a eu recours à la conteneurisation.

La conteneurisation

Il s'agit d'une forme de virtualisation du système d'exploitation dans laquelle on exécute des applications dans des espaces utilisateurs isolés appelés conteneurs,
qui utilisent le même système d'exploitation partagé.
Un conteneur d'applications est un environnement informatique entièrement regroupé en package et portable :

    il dispose de tout ce dont une application a besoin pour s'exécuter (y compris ses fichiers binaires, bibliothèrques, dépendances et fichiers de configuration, le tout encapsulé et isolé dans un conteneur).
    la conteneurisation d'une application permet d'isoler le conteneur du système d'exploitation hôte, avec un accès limité aux ressources sous-jacentes, comme une VM légère.
    on peut exécuter l'application conteneurisée sur différents types d'infrastructure, tels qu'un serveur "bare metal", dans le cloud ou sur des VM, sans avoir à la remanier pour chaque environnement.

La conteneurisation permet de réduire les charges au démarrage et de supprimer la nécessité de configurer des systèmes d'exploitation invités distincts pour chaque application.
=> Ils partagent tous un seul noyau de système d'exploitation.

Utilité

Cela a permis aux développeurs de logiciels de créer et déployer des applications de façon plus rapide et sécurisée.

Docker a permis de rendre la conteneurisation pratique, efficace et populaire.
Les conteneurs offrent plusieurs avantages : voir cet article

    portabilité
    vitesse
    évolutivité
    agilité
    efficacité
    isolation
    sécurité
    facilité de gestion
    continuité
    facilité d'utilisation pour les développeurs

Docker et la conteneurisation ont été la réponse à une série de défis persistants dans le monde de l'informatique.
Ce procédé (combinaison de reproductibilité, protabilité et efficacité) à offert une solution aux problèmes que les méthodes traditionnelles résolvaient avec difficulté.

Concepts clés

    Image :
    Une image est l'élément de base d'un conteneur. Il s'agit d'un ensemble de fichiers systèmes et de paramètres d'exécutions.
    Les fichiers peuvent être les suivants : code source, bibliothèques, dépendances, l'environnement d'exécution (ex JVM), les pilotes, outils, scripts + autres fichiers nécessaires à l'exécution de l'application.
    Une image n'a pas d'état et est exécutée par la runtime docker. On peut la voir comme une description d'un conteneur.
    On peut créer une image soit en modifiant un conteneur déjà en marche (sorte de snapshot d'un conteneur), soit en créant un Dockerfile basé sur une image qui existe déjà.
    Conteneur :
    Un conteneur est une instance en exécution d'une image. Il s'agit d'une encapsulation légère d'une application et de son environnement d'exécution.
    Le conteneur fonctionne de manière isolée sur le système hôte.

Attention, différence entre une image docker et un conteneur (sont pas la même chose) :

    L'image docker contient tous les éléments nécessaires à l'exécution d'un logiciel : le code, un environnement d'exécution (ex JVM), les pilotes, les outils, scripts, bibliothèques, etc.
    Une image n'est pas modifiable. Si on souhaite la modifier, il faut en créer une nouvelle.
    On stockera les images dans le "registry" afin de pouvoir les télécharger.
    Le conteneur est une sorte de mode d'emploi pour l'utilisation de l'image. Les conteneurs ne sont pas persistants et sont lacés à partir d'images.

    Dockerfile :
    Il s'agit d'un fichier texte (ou fichier script) qui décrit une image.
    Il est constitué d'instructions qui servent à décrire une image Docker et il contient les instructions pour construire une image Docker.
    Le dockerfile permet d'automatiser la "build" (création) d'images.
    Entrypoint :
    Le point d'entrée d'un conteneur. Il s'agit d'une commande qui se lance au démarrage du conteneur.
    La plupart des images officielles Docker sont configurées avec un entrypoint /bin/bin ou /bin/bash.
    Registry :
    Le registre est une zone de stockage pour les images Docker (public ou privé).
    Docker Hub est un service cloud public pour partager et stocker des images Docker.
    C'est l'équivalent d'un GitHub pour les images Docker.
    Volume :
    Le Volume est utilisé pour faire persister les données et partager des fichiers entre le conteneur et l'hôte.
    Les volumes sont essentiels pour éviter la pete de données lorsque les conteneurs sont arrêtés ou supprimés.
    C'est aussi une solution pratique pour l'échange de données entre l'hôte et le conteneur.
    Le volume est hébergé par l'ordinateur hôte, en dehors du véritable conteneur.
    Il faut voir le volume comme un dossier qui est partagé entre le conteneutr et l'ordinateur hôte.
    Plusieurs conteneurs peuvent aussi se partager un même volume.

Statefull vs Stateless app :

Un processus ou une application stateless est indépendant. Il ne stocke pas de données et ne fait référence à aucune transaction passée.
Chaque transaction est effectuée à partir de rien, comme si c'était la première fois. Les applications stateless fournissent un service 
ou une fonction et utilisent un réseau de diffusion de contenu, le web ou des serveurs d'impression pour traiter ces requêtes à court terme.

Les applications et processus stateful, quant à eux, peuvent être réutilisés indéfiniment. 
Les plateformes bancaires en ligne et les messageries en sont deux exemples. 
Les transactions précédentes sont prises en compte et peuvent affecter la transaction actuelle. 
C'est pour cela que les applications stateful utilisent les mêmes serveurs chaque fois qu'elles traitent une requête 


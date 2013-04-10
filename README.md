ubuntu-cafe
===========

My experience on creating customizing my ubuntu installation for a cybercafe

Some links to share Wifi with per-user authentication
http://askubuntu.com/questions/180733/how-to-setup-a-wi-fi-hotspot-access-point-mode
http://ubuntuforums.org/showthread.php?t=151781 & http://ubuntuforums.org/showthread.php?t=151782
http://wiki.freeradius.org/guide/SQL%20HOWTO#Create-MySQL-Database
http://wiki.freeradius.org/guide/Dialup-admin
http://lists.freeradius.org/pipermail/freeradius-users/2010-October/049518.html

http://doc.ubuntu-fr.org/coovachilli (not used)

French version :
Quelques scripts que j'ai utilisé ou créé pour adapter Ubuntu 12.04 à une utilisation dnas un cybercafé.

Il y a des scripts purement système et d'autres qui sont du domaine de la "bidouille" pour gérer des dossiers utilisateurs (RàZ à la déconnexion, ...). Je décris en premier l'objectif et les manipulations hors scripts, réalisées une seule fois.

L'objectif est de faire tourner plusieurs machines ("stations") avec une installation commune située sur une machine principale.
La machine principale héberge donc 2 installations : celle qu'elle partage et la sienne : elles sont en partie communes car il s'agit presque des mêmes, pour réduire l'espace disque utilisé (j'ai installé sur un SSD, c'est plus performant).
La racine de l'installation partagée est dans un dossier nommé nfsroot situé lui-même à la racine de l'installation de la machine principale. Ce dossier contient un dossier usr qui est partagé avec l'installation principale : ce dossier n'est pas modifié en utilisation "normale", mais uniquement lors des mises à jour. Il est monté sur /usr pour le partage (créer unlien symbolique gêne VirtualBox). Les deux installations sont configurées en désactivant les recherches de mises à jour car celles-ci pourraient perturber l'installation en cas de déclenchement parallèle étant donné le partage du dossier usr, modifié par les mises à jour.
Le cache des paquets .deb est aussi partagé, inutile de télécharger 2 fois les mises à jour, ici pas de point de montage, un lien symbolique suffit.

Scripts et paramétrages "systèmes" (en partant de la couche noyau)
Amorçage via le réseau avec TFTP (netboot)
Paramétrage de la configuration servant à Ubuntu pour générer un fichier initrd gérant le boot via le réseau (NFS)

Script "rootaufs" pour avoir un pseudo système de fichier reposant sur une partie en lecture seule (partagée) et une partie en écriture ré-initialisée pour que les statiosn puissent écrire (Ubuntu semble créer des modifications à certains endroits dans l'arborescence et il est possible qu'il soit perturbé s'il ne parvient pas à écrire...)

Répertoire racine TFTP
Il est basé sur une image pour installer Ubuntu via netboot : ajout de 2 entrées

Paramétrage des partage NFS

Installation de DNSMASQ au sein de l'installation principale qui fait office de "couteau suisse" ici : on utilise les fonctions de relais DHCP, cache DNS et enfin serveur TFTP (il fait tout ça!, ça évite de configurer un "gros" serveur DHCP, un "gros" bidule DNS, ...)
Il faut configurer ce bel outils

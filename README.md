# Trancendances Online Festival

## Introduction

Afin de célébrer les 5 ans d'existence du projet qu'elle porte, l'association Trancendances a décidé d'organiser le Trancendances Online Festival. L'objectif est de reprendre les codes des plus grands festivals de musique électronique autour du monde (EDC, A State Of Trance, Electronic Family...) et de les transposer sur Internet afin de créer un événement unique en son genre, un festival se déroulant entièrement en ligne afin de profiter pleinement de l'aspect communautaire et participatif de ce média, ainsi que de ses valeurs d'échange et de partage.

> « L'objectif est de créer quelque chose d'inédit, explique le président de Trancendances, Brendan Abolivier, au journal [Le Télégramme](http://www.letelegramme.fr/finistere/brest/web-trancendances-online-festival-ce-soir-30-09-2016-11236823.php). Internet est un formidable outil d'échange et de partage qui nous permet de populariser la musique trance et de la rendre plus accessible.»

Ainsi, le 30 septembre 2016, Trancendances ouvre, pour une durée de 5h, trois salles virtuelles simultanées, chacune apportant son univers et son style aux travers d'une line-up triée sur le volet, et constituée d'artistes mélangeants petits noms méritant plus de reconnaissance et têtes d'affiche à la réputation ayant fait le tour du monde.

## Infrastructure

Le Trancendances Online Festival a nécessité une infrastructure informatique composée de 11 serveurs (détaillés ci-dessous), hébergés sur le [Public Cloud d'OVH](https://www.ovh.com/fr/cloud/instances/).

### Les trois salles

Chaque salle du festival est constituée de trois serveurs :

* Un serveur [MPD](https://musicpd.org/) permettant d'ordonner et contrôler la lecture des différents fichiers audios composant la salle
* Deux serveurs [Icecast](http://www.icecast.org/) relayant le flux audio

Le serveur MPD se positionne donc en recul. Il contient les DJ sets et autres éléments audios ordonnés par une playlist. Contrôlé par le client MPD [mpc](https://musicpd.org/clients/mpc/), il envoi de façon automatisée (grâce à une tâche cron) le flux audio correspondant à sa salle aux deux serveurs Icecast. Ces deux serveurs sont chacun une copie de l'autre (à l'exception du nom d'hôte), afin de permettre une répartition de la charge.

### La *landing page*

Lorsqu'un auditeur accède à *festival.trancendances.fr* (adresse actuellement indisponible) dans son navigateur, il arrive sur une *landing page* contenant un lecteur audio, une infographie rendant compte de la programmation du festival, ainsi qu'une iframe vers un webchat IRC.

Le serveur hébergeant ce site utilise le serveur Web [Caddy](https://caddyserver.com). Une copie a également été téléversée sur un conteneur *[object storage](https://www.ovh.com/fr/cloud/storage/object-storage.xml)* afin de rediriger le traffic en cas de défaillance du serveur.

Le contenu de cette *landing page* est disponible dans le répertoire `web` de ce dépôt.

### Le proxy

Afin d'éviter une surcharge des serveurs Icecast et Caddy, un proxy frontal a été installé. Son rôle principal est d'effectuer une répartition de charge entre les deux serveurs Icecast de chaque salle d'une part, et d'opérer une redirection de secours vers le conteneur *object storage* en cas de défaillance du serveur Web.

Il avait été décidé d'utiliser le répartiteur de charge [HAproxy](http://www.haproxy.org/) pour s'occuper de cette partie de l'infrastructure. Cependant, à cause d'un disfonctionnement de dernière minute de ce dernier, un serveur Caddy a été installé et configuré afin de le remplacer.

Sa configuration est disponible dans le fichier `proxy.caddyfile` de ce dépôt.

## À propos de Trancendances

[![](https://cloud.githubusercontent.com/assets/5547783/16178421/7f568a30-3647-11e6-891d-5e14384425e4.png)](https://www.trancendances.fr)

Trancendances est une association française loi 1901 à but non lucratif travaillant à populariser la musique trance en France. Avec plus de 5 ans d'expérience dans ce genre musical et la scène internationnale qui l'entoure, nous avons eu l'occasion de prendre part à l'organisation de grands événements français (Wackii Time Party, Fables Festival...), travailler avec les meilleurs labels et artistes autour du monde, et promouvoir cette musique au travers de nos podcasts, reviews, articles et concours.

Un des aspects de la gestion de Trancendances qui nous tient le plus à cœur est la transparence, au même titre que le respect de ses utilisateurs (absence de pistage sauf extrême nécessité) et l'utilisation de logiciels libres en accord avec leur philosophie.


## Nous contacter

Il y a plein de façons de nous contacter, que ce soit par [e-mail](mailto:oss@trancendances.fr), [Twitter](https://twitter.com/Trancendances) ou [Facebook](https://facebook.com/Trancendances), ou même dans les [issues](https://github.com/Trancendances/online-festival/issues) de ce dépôt.

Vous pouvez également contacter le responsable de ce dépôt à <brendan@trancendances.fr>.

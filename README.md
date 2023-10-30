# Born2beRoot

Ce projet est conçu pour vous initier au monde de la virtualisation et à la configuration d'un système d'exploitation basé sur Linux.

## Table des Matières

- [Préambule](#préambule)
- [Introduction](#introduction)
- [Consignes Générales](#consignes-générales)
- [Partie Obligatoire](#partie-obligatoire)
- [Partie Bonus](#partie-bonus)
- [Rendu et Peer-Evaluation](#rendu-et-peer-evaluation)

## Préambule

Bienvenue dans ce projet Born2beRoot. 

## Introduction

L'objectif est de vous familiariser avec la virtualisation et l'administration système en installant et en configurant votre propre serveur.

## Consignes Générales

1. Utilisez VirtualBox (ou UTM si VirtualBox n'est pas compatible avec votre système).
 - `sudo apt install VirtualBox`
2. Le rendu se fera sous forme d'un fichier `signature.txt`, contenant la signature du disque virtuel de votre machine. 

## Partie Obligatoire

1. **Installation du Système d'Exploitation**: Choisissez entre Debian (version stable) et Rocky (version stable). Debian est recommandé pour les débutants.
2. **Configuration de Base**:
   - Installez le serveur sans interface graphique.
   - Créez au minimum 2 partitions chiffrées en utilisant LVM.
   - Configurez le nom de l’hôte de votre machine avec votre login suivi de 42 (exemple : login42).
   - Créez un utilisateur avec votre login et assurez-vous qu'il appartienne aux groupes `user42` et `sudo`.
3. **Configuration SSH**:
   - Activez le service SSH sur le port 4242.
   - Désactivez la connexion SSH en tant qu'utilisateur root.
4. **Configuration du Pare-feu**:
   - Installez et configurez UFW (ou firewalld pour Rocky) pour n'autoriser que le port 4242.
5. **Politique de Mot de Passe**:
   - Mettez en place une politique de mot de passe fort conformément aux exigences du projet.
6. **Configuration de Sudo**:
   - Configurez sudo selon une pratique stricte, conformément aux exigences du projet.

## Partie Bonus

> **Remarque**: Les bonus ne seront évalués que si la partie obligatoire est complètement réussie.

1. **Partitions Supplémentaires**: Configurez des partitions supplémentaires pour correspondre à la structure suggérée dans le projet.
2. **Site Web WordPress**: Mettez en place un site WordPress fonctionnel en utilisant lighttpd, MariaDB et PHP.
3. **Service Supplémentaire**: Installez et configurez un service supplémentaire de votre choix (NGINX/Apache2 sont exclus).

## Rendu et Peer-Evaluation

1. Générez la signature de votre disque virtuel en suivant les instructions fournies dans le projet.
2. Déposez le fichier `signature.txt` à la racine de votre dépôt Git.

> **Attention**: Ne soumettez pas votre machine virtuelle sur le dépôt Git.

## Questions pour la Soutenance

Soyez prêt à répondre aux questions sur les sujets suivants lors de la soutenance :

- La différence entre aptitude et apt.
- Le fonctionnement de SELinux ou AppArmor, selon le système d'exploitation que vous avez choisi.
- Le fonctionnement du service SSH.
- Comment fonctionne sudo et comment il est configuré sur votre système.
- La politique de mot de passe mise en place.
- Comment fonctionne le script monitoring.sh et comment arrêter son exécution.


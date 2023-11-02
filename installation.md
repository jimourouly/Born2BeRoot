
# Guide d'Installation pour Born2beRoot

Ce document fournit des instructions détaillées étape par étape pour l'installation et la configuration de votre serveur dans le cadre du projet Born2beRoot.

## Table des Matières

1. [Prérequis](#prérequis)
2. [Installation du Système d'Exploitation](#installation-du-système-dexploitation)
3. [Configuration de Base](#configuration-de-base)
4. [Configuration SSH](#configuration-ssh)
5. [Configuration du Pare-feu](#configuration-du-pare-feu)
6. [Politique de Mot de Passe](#politique-de-mot-de-passe)
7. [Configuration de Sudo](#configuration-de-sudo)
8. [Partie Bonus (Optionnelle)](#partie-bonus-optionnelle)
9. [Vérification de la Configuration](#vérification-de-la-configuration)

## Prérequis

- Avoir installé VirtualBox ou UTM. (sur les machines dans les clusters, VirtualBox est deja installe)
`sudo apt install VirtualBox`
- Télécharger l'image ISO de Debian.
https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/



## Installation du Système d'Exploitation

1. Créer une nouvelle machine virtuelle dans VirtualBox/UTM.
2. Sélectionnez le type de système d'exploitation Linux.
3. Attribuez au moins 2 Go de RAM à la machine virtuelle.
4. Créez un disque virtuel de taille suffisante (~12 Gb).
5. Démarrez la machine virtuelle et sélectionnez l'image ISO téléchargée pour l'installation.
6. Suivez les instructions d'installation en choisissant les options appropriées.

## Installation OS Debian

1. Sélectionnez l'option d'installation sans interface graphique. `install`
2. Choisissez la langue voulue, (de preference `English`)
3. La localisation.
4. Keyboard Layout, le layout de votre machine.
5. Pour les informations d'identification voir ci-dessous : [Informations identifications](#information-d'identifications)
6. Partitions Disks:
   - Sélectionnez `guided - Use entire ... encrypted LVM`.
   - Selectionner le disque `SCI3` (Disque entier)
   - Lors de la partition, utilisez `Separate /home, /var...`
   - Valider les changements sur le dique.
   - Creer un passphrase pour l'encriptage du disque.
	   - [Informations identifications](#information-d'identifications)
	- Choisissez la taille max du disque en ecrivant `max`
	- Confirmer `finish patritionning and write change to disk`
7. Configure package manager :
	- Refusez de `Scan another CD or DVD`
	- Pour le mirroir Debian, selectionnez le pazs ou vous vous trouver.
	- choisissez `deb.debian.org`
	- Laissez blanc le proxy.
8. Refusez le survey
9. Enlevez toutes les coches. `SSH Server` et `Standard system utilities`
10. Installez le GRUB `to the master boot record`
11. Selectionnez le disque `/dev/sda`

L'OS va finaliser l'installation. 
Bravo Loris tu as reussi. 
 

## information d'identifications
Voici les informations a mettre lors de la creation des comptes :

	- Hostname = Login+42. Exemple "jroulet42"
	- Username full name = "jroulet_full"
	- Login = login 42. Exemple "jroulet"
	 - Pensez a mettre un mot de passe identique pour le root, le login et le passphrase. 


## Configuration de Sudo

1. Installez sudo si ce n'est pas déjà fait : `apt install sudo`.
2. Éditez le fichier de configuration sudo : `sudo visudo`.
3. Ajoutez les configurations requises pour une utilisation stricte de sudo (voir les exigences du projet).
`"Les paths utilisables par sudo seront restreints, là encore pour des questions de
sécurité. Exemple :
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"`



## Configuration SSH

1. Installez le serveur SSH si ce n'est pas déjà fait : `sudo apt install openssh-server`.
2. Éditez le fichier de configuration SSH : `sudo vim /etc/ssh/sshd_config`.
(Si Vim n'est pas installe utilisez nano)
3. Changez le port par défaut en 4242 : `Port 4242`.
4. Désactivez la connexion en tant que root : `PermitRootLogin no`.
5. Redémarrez le service SSH : `sudo systemctl restart ssh`.

##Tester la configuration SSH

1. Fermer la VM.
2. Allez dans les parametres de la VM sur VirtualBox.
3. Ouvrez les settings, Network, Adaptater 1.
4. Cliquez sur Advanced, puis Port Forwarding.
5. Cliquez sur le "+" vert. Laissez Rule 1, changez Host port et Guest port en "4242".
6. Quittez les settings. 
7. Redemarrez la vm.

8. Avec le teminal connectez vous en ssh en tapant `ssh lgaume@127.0.0.1 -p 4242`.
Changer lgaume par votre nom d'utilisateur.

9. Sur la vm, ecrivez `sudo service sshd status`.
Vous devriez voir la connexion qui a ete faite. 

## Configuration du Pare-feu

1. Installez UFW (ou firewalld pour Rocky) : `sudo apt install ufw` (ou `sudo dnf install firewalld` pour Rocky).
2. Activez UFW : `sudo ufw enable`.
3. Autorisez le port 4242 : `sudo ufw allow 4242/tcp`.
4. Vérifiez la configuration : `sudo ufw status`.

## Politique de Mot de Passe

1. Installez le package `libpam-pwquality` : `sudo apt install libpam-pwquality`.
2. Éditez le fichier de configuration PAM : `sudo vim /etc/pam.d/common-password.
3. Ajoutez vos règles pour la complexité du mot de passe (Voir https://linux.die.net/man/8/pam_pwquality).
4. indice : retry, minlen, ucredit, dcredit ... 
<details>
	<summary>Regle a modifier Spoiler</summary>

1. Trouvez la ligne qui commence par `password requisite pam-pwquality.so`.
2. Ajoutez a la suite `retry=3 minlen=10 ucredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root`.

</details>

## Création Groupe 

#TODO 

## Partie Bonus (Optionnelle)

### Partitions Supplémentaires

1. Créez et configurez des partitions supplémentaires selon la structure suggérée dans le projet.

### Site Web WordPress

1. Installez lighttpd, MariaDB, et PHP.
2. Configurez une base de données pour WordPress.
3. Téléchargez et configurez WordPress.
4. Assurez-vous que le site est accessible et fonctionne correctement.

### 1. Installer Lighttpd, MariaDB et PHP
`bash
sudo apt update
sudo apt install lighttpd mariadb-server php php-cgi php-mysql
`

### 2. Activer PHP pour Lighttpd
`bash
sudo lighty-enable-mod fastcgi-php
sudo systemctl restart lighttpd
`

### 3. Sécuriser MariaDB
`sudo mysql_secure_installation`

### 4. Configurer une Base de Données pour WordPress
`bash
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
exit;
`

### 5. Télécharger et Configurer WordPress

1. Deplacement dans le dossier 
`cd /var/www/html`

2. Téléchargement de WordPress
`sudo wget https://wordpress.org/latest.tar.gz`

3. Extraction des Fichiers WordPress
`sudo tar -xzvf latest.tar.gz`

4. Attribution des Permissions
`sudo chown -R www-data:www-data /var/www/html/wordpress`

5. Création du Fichier de Configuration WordPress
`sudo mv /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php`

6. Édition du Fichier de Configuration WordPress
`sudo vim /var/www/html/wordpress/wp-config.php`

Assurez-vous de configurer les paramètres de la base de données dans le fichier wp-config.php.

### 6. Configurer Lighttpd pour WordPress
1. activation de la modification
`sudo lighty-enable-mod rewrite`

2. Restart du service
`sudo systemctl restart lighttpd`

## Vérification de la Configuration

### 1. Vérifier la Configuration de Lighttpd
1. Verification si lighttpd est actif
`sudo systemctl status lighttpd`

Assurez-vous que le service est actif et en cours d'exécution.

### Service Supplémentaire

1. Choisissez un service supplémentaire à installer et à configurer.

## Vérification de la Configuration


1. Testez la connexion SSH sur le port 4242.
`ssh -p 4242 votre_utilisateur@votre_serveur`

3. Vérifiez que le pare-feu est configuré correctement.
`sudo ufw status`

4. Assurez-vous que la politique de mot de passe est appliquée.
`cat /etc/security/pwquality.conf`

5. Vérifiez la configuration de sudo.
`cat /etc/sudoers`

---

Une fois que tout est configuré, vous pouvez revenir au [README.md](README.md) pour les étapes finales et les instructions pour le rendu.



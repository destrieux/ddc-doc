# Prérequis à l'installation de CiviCRM et de l'extension don du corps
## Matériel
L'application est utilisée sur une machine ayant les caractéristiques suivantes : 

* Machine virtuelle, 2 CPU,
* 16 Go de RAM,
* 16 Go de disque ; à augmenter en fonction du nombre de documents qui sont stockés sur le serveur (scan des promesses de don, documents générés...),
* Debian récente, 11 ou supérieure,
* Serveur Apache,
* PHP version 8 ou supérieure.

## Installation du serveur LAMP
### PHP
Pour installer PHP et les paquets demandés par CiviCRM : <br>
```apt-get install php composer libapache2-mod-php php-pear php-cgi php-common php-mbstring php-zip php-net-socket php-gd php-xml-util php-mysql php-bcmath php-intl php-imagick unzip wget curl git -y```

Bien vérifier que les fonctions d'internationalisation de PHP sont activées : <br>
la ligne ```;extension=intl``` doit être décommentée dans **les deux** fichiers : 

* /etc/php/VERSION/apache2/php.ini
* /etc/php/VERSION/cli/php.ini

### Mariadb

* Installer Mariadb (au moins v. 10.5 sinon certaines requêtes ne marchent pas) et phpmyadmin <br>
```apt-get install mariadb-server mariadb-client phpmyadmin -y```

* Démarrer Mariadb <br>
```systemctl start mariadb```

* Pour faire démarrer Mariadb à chaque reboot <br>
```systemctl enable mariadb```

* Sécuriser madiadb <br>
```mysql_secure_installation`` <br>
Entrez les informations : mdp root Mariadb, suppression utilisateurs anonymes, désactiver accès root distants, supprimer base test :

    * Enter current password for root (enter for none):
    * Change the root password? [Y/n] Y
    * New password: <MARIADBPASSWD>
    * Remove anonymous users? [Y/n] Y
    * Disallow root login remotely? [Y/n] Y
    * Remove test database and access to it? [Y/n] Y
    * Reload privilege tables now? [Y/n] Y

* Mettre en place la timezone <br>
```mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root mysql```

* Redémarrer Mariadb <br>
```systemctl restart mariadb```



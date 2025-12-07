# Prérequis à l'installation de CiviCRM et de l'extension don du corps
## Matériel
L'application est utilisée sur une machine ayant les caractéristiques suivantes : 

* Machine virtuelle, 2 CPU,
* 16 Go de RAM,
* 16 Go de disque ; à augmenter en fonction du nombre de documents qui sont stockés sur le serveur (scan des promesses de don, documents générés...),
* Debian récente, 11 ou supérieure,
> A partir de Trixie (debian13), le paquet unoconv n'est plus maintenu. Si vous utilisez une version supérieure à 12, vous devez [l'installer manuellement](../civicrm/cv.md). 
* Serveur Apache,
* PHP version 8 ou supérieure.
* Mariadb 11.8.3

## Installation du serveur LAMP
### sudo
Installer sudo: <br>
```su -s```
```apt-get install sudo```

### PHP
Pour installer PHP et les paquets demandés par CiviCRM : <br>
```apt-get install php composer libapache2-mod-php php-pear php-cgi php-common php-mbstring php-zip php-net-socket php-gd php-xml-util php-mysql php-bcmath php-intl php-imagick unzip wget curl git -y```

Bien vérifier que les fonctions d'internationalisation de PHP sont activées : <br>
la ligne ```;extension=intl``` doit être décommentée dans **les deux** fichiers (supprimer le ; initial): 

* /etc/php/VERSION/apache2/php.ini
* /etc/php/VERSION/cli/php.ini

### Mariadb

* Installer Mariadb (au moins v. 10.5 sinon certaines requêtes ne marchent pas) et phpmyadmin <br>
```apt-get install mariadb-server mariadb-client phpmyadmin -y```
Lors de l'installation, choisir apache2 et création automatique d'un mdp pour phpmyadmin

* Démarrer Mariadb <br>
```systemctl start mariadb```

* Pour faire démarrer Mariadb à chaque reboot <br>
```systemctl enable mariadb```

* Mettre en place la timezone <br>
```mariadb-tzinfo-to-sql /usr/share/zoneinfo | mysql -u root mysql```

* Redémarrer Mariadb <br>
```systemctl restart mariadb```

## Installation d'un montage automatique samba
CiviOffice est une extension de CiviCRM qui génère les documents (courriers, cartes...) à partir de [modèles modifiables par l'utilisateur](../../Parametrage/user.md) avec un systeme de jetons remontant des informations de la base de données.

Le plus pratique est de disposer d'un volume partagé, modifiable par l'utilisateur et visible par le serveur. Vous pouvez par exemple créer un partage samba sur un serveur de fichier (par exemple *//monserveur.univ-XXX.fr/civicrm *) qui exportera un répertoire *civicrm* vers votre serveur CiviCRM.

### Installer le client samba :

    apt-get install samba

### Créer un montage automatique 
Le répertoire civicrm exporté par monserveur.univ-XXX.fr doit être accessible à l'utilisateur *civicrm* ayant *civicrm_SMPPWD* comme mot de passe, dans le domaine DOMAIN. 

Ces informations sont stockées dans un fichier *.smbcreds* seulement lisible par root : 

    nano /etc/samba/.smbcreds

    username=civicrm
    password=civicrm_SMPPWD
    domaine=DOMAIN

    chmod go-rwx /etc/samba/.smbcreds

Créer un point de montage */mnt/smb*

    mkdir /mnt/smb

Créerune unité systemd qui montera le volume à la demande sur le point de montage */mnt/smb*. Il faut créer deux fichiers pour le montage mannuel et le montage automatique : 

> ATTENTION : le nom des deux ficher dépend du point de montage. Si celui-ci est /mnt/smb, les fichers **doivent** être nommés *mnt-smb.mount* et *mnt-smb.automount* (le chemin vers ces fichiers en remplaçant "/" par "-".)

**Pour le montage manuel**

    nano /etc/systemd/system/mnt-smb-sambashare.mount

    [Unit]
    Description=partage
    StartLimitIntervalSec=0

    [Mount]
    Type=cifs
    What=//monserveur.univ-XXX.fr/civicrm
    Where=/mnt/smb/sambashare
    Options=uid=0,credentials=/etc/samba/.smbcreds,iocharset=utf8,vers=3.0
    TimeoutSec=10

    [Install]
    WantedBy=multi-user.target

> *What* : ce qui est monté, *Where* : le point de montage, *Options* : notamment les identifiants

**Pour le montage automatique**

    nano /etc/systemd/system/mnt-smb-sambashare.automount

    [Unit]
    Description=samba automount for yourfiles

    [Automount]
    Where=/mnt/smb/sambashare


    [Install]
    WantedBy=multi-user.target

> Les deux fichiers doivent être créés.

**Recharger le service**

    systemctl daemon-reload

**Activation de l'unité automount**

    systemctl enable --now mnt-smb-sambashare.automount

Pour vérifier qu'elle est active : 

    systemctl

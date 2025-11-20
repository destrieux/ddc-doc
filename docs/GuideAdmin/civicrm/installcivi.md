# Installation de CiviCRM
## Créez la base de données 
CiviCRM utilise une base de données qui peut être commune avec Wordpress.
Néanmoins, il est préférable de créer une base distincte, par exemple *civicrm*.

* Créez la base civicrm <br>
```mysql -u root -p -e "CREATE DATABASE civicrm DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci"```

* Donnez tous les droits à l'utilisateur civicrm ayant le mot de passe *MDPCIVICRM*<br>
```mysql -u root -p -e "grant all on civicrm.* to 'civicrm'@'localhost' identified by 'MDPCIVICRM'"```

## Installer les fichiers CiviCRM
La [documentation officielle est disponible ici](https://docs.civicrm.org/sysadmin/en/latest/upgrade/wordpress/).

* Récupérez le [fichier d'installation pour Wordpress civicrm-X.XX.X-wordpress.zip disponible sur le site civiCRM](https://civicrm.org/fr/telecharger). <br>

* Déplacez et dézippez l'archive téléchargée dans les pluggins de Wordpress <br>
```cp civicrm-X.XX.X-wordpress.zip /var/www/html/ddctest/wp-content/plugins```<br>
```cd /var/www/html/ddctest/wp-content/plugins```<br>
```unzip civicrm-X.XX.X-wordpress.zip``` <br>

> Les versions CiviCRM > 6 incluent les fichiers linguistiques qu'il n'est plus nécessaire de traiter séparément

* Changer les permissions de CiviCRM <br>
```chown -R www-data:www-data /var/www/html/ddctest/wp-content/plugins/civicrm``` <br>

## Lancer l'installeur CiviCRM
* Connectez-vous à l'interface d'administration du site Wordpress <br> ```https://ddctest/wp-admin/``` avec des permissions admin. <br>
```identifiant : USERWORDPRESS``` <br>
```password : MDPUSERWORDPRESS``` <br>

* Allez dans les extensions de Wordpress et activez CiviCRM pour lancer l'installation <br>
```Langue: français``` <br>
Modifier la base civicrm : ```mysql://civicrm:<MDPCIVICRM>@localhost/civicrm```


## Si le site Wordpress devient inaccessible après l'installation de CiviCRM
L'installation peut parfois rendre Wordpress inaccessible  en raison d'une erreur d'installation.

Il suffit alors de désactiver CiviCRM pour relancer l'installation après avoir réglé le problème. 

* Connectez-vous sur la base Wordpress via Phpmyadmin, <br>
* Dans la table ```wp_options```, colonne ```option_name```, cherchez la ligne ```active plugins``` <br>
changer la valeur pour ```a:0:{}``` <br>
* Rechargez la page admin de Wordpress.


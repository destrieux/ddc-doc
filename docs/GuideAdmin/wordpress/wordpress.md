# Installation de Wordpress
> Ne pas utiliser le paquet Debian Wordpress qui s'installe dans une arborescence différente. 
## Création de la base de données

* Créez un fichier de configuration qui va générer une base de données pour Wordpress avec le mot de passe *WORDPRESSPWD* <br>
```nano ~/wp.sql```<br>

        CREATE DATABASE wordpress;
        GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
        ON wordpress.*
        TO wordpress@localhost
        IDENTIFIED BY 'WORDPRESSPWD';
        FLUSH PRIVILEGES;

* Créez la base de données <br>
```cat ~/wp.sql | mysql --defaults-extra-file=/etc/mysql/debian.cnf```
Normalement, vous pouvez accéder à la base wordpress depuis 
```http://YOURHOST/phpmyadmin```
```user : wordpress````
```password : WORDPRESSPWD```

## Préparez l'installation de Wordpress

* Téléchargez Wordpress et installez-le dans le répertoire *ddctest*,<br>
```cd /var/www/html``` <br>
```wget https://wordpress.org/latest.zip``` <br>
```unzip latest.zip``` <br>
```mv wordpress ddctest``` <br>

> Attention pas de caractères spéciaux dans le nom du site !

* Éditez le ficher de configuration *wp-config.php* utilisé par Wordpress lors de l'installation, <br>
```cd ddctest``` <br>
```cp wp-config-sample.php wp-config.php``` <br>
```nano wp-config.php``` <br>

        /** The name of the database for WordPress */
        define( 'DB_NAME', 'wordpress' );
        /** MySQL database username */
        define( 'DB_USER', 'wordpress' );
        /** MySQL database password */
        define( 'DB_PASSWORD', 'WORDPRESSPWD' );
        /** MySQL hostname */
        define( 'DB_HOST', 'localhost' );

* Changez les permissions <br>
```chown -R www-data:www-data /var/www/html/ddctest```
## Créez le site
* Déclarez le site,
```cd /etc/apache2/sites-available```<br>
```cp 000-default.conf ddctest.conf```<br>
```nano ddctest.conf```<br>

        change : DocumentRoot /var/www/html/ddctest
        change : ServerAdmin mailadmin@domaine.fr

* Désactivez l'hôte virtuel par défaut, activez le site et relancez le serveur web. <br>
```a2dissite 000-default``` <br>
```a2ensite ddctest``` <br>
```service apache2 reload``` <br>

## Installez Wordpress
* dans un navigateur : ```http://HOSTNAME/wp-admin```<br>
with HOSTNAME : hostname or IP

* Entrez les informations demandées :

    * langue,
    * titre du site : ddctest,
    * identifiant : USERWORDPRESS,
    * password : MDPUSERWORDPRESS.
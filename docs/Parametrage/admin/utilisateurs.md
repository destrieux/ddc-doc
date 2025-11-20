# Créer les utilisateurs

## Créer des utilisateurs dans Wordpress
**Menu Wordpress > Comptes > Ajouter un Compte** <br>
Par privilège décroissant : 

* administrateurs (tous les droits), 
* auteurs, 
* contributeurs.

## Les récupérer dans CiviCRM
**Administration > utilisateurs et permissions > synchroniser**. <br>
Crée un utilisateur CiviCRM par utilisateur Wordpress.

## Régler les permissions
Les permissions dans CiviCRM sont gérées par le statut dans Wordpress :

* admin : ont tous les droits
* auteurs : gestionnaires dans CiviCRM
* contributeurs : visu sans modification dans CiviCRM

Les permissions sont réglées finement dans : <br>
**Administration > Utilisateurs et permissions > Permissions > Wordpress control access**.
> Elles sont normalement correctement réglées à l'installation de l'extension Don du Corps et vous n'avez pas besoin de les modifier.

Les permissions type sont :
![permissions type](../../img/Reglage_ACL_Civicrm.png)

## Limiter les répertoires lisibles
Si les permissions d'accès aux fichiers de /var/www sont trop larges, vous aurez un avertissement de sécurité.

Pour éviter ce problème, activez [.htaccess]( https://civicrm.org/advisory/civi-sa-2014-001-risk-information-disclosure)

```nano /etc/apache2/apache2.conf```

* Naviguez jusqu'aux lignes suivantes <br>
```<Directory /var/www/>``` <br>
```Options Indexes FollowSymLinks``` <br>
```AllowOverride None``` <br>
```Require all granted``` <br>
```</Directory>``` <br>

* Changez : <br>
```AllowOverride None``` par ```AllowOverride All```

* Relancez Apache2 <br>
```systemctl restart apache2```


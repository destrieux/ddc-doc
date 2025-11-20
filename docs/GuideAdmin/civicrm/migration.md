# Migration de la base de données CiviCRM
Vous pouvez avoir besoin de migrer une installation de CiviCRM vers une autre machine. <br>
Il s'agit d'une migration complète de la base.
## Prérequis
Important : les versions CiviCRM des serveurs source et cible :

* DOIVENT ÊTRE IDENTIQUES (le format des bases change avec la version),
* Avec les mêmes extensions.

## Sur le serveur source
Exportez la base vers un fichier *civicrm_sourcebase.sql* <br>
````mysqldump -u civicrm_source_base_user -p civicrm_source_base > civicrm_sourcebase.sql````

Avec :

* *civicrm_source_base* : base à exporter,
* *civicrm_source_base_user* : utilisateur de la base à exporter,
* *civicrm_sourcebase.sql* : nom du fichier d'export.

## Sur le serveur cible
* Désactiver CiviCRM : **Wordpress > Extensions > CiviCRM** : désactiver,
* Dans PhpMyadmin : supprimer toutes les tables de la base civicrm,
* Importer la base sauvegardée à l'étape précédente :<br>
```mysql -u civicrm -p``` (se connecter à mysql avec le nom d'utilisateur défini à l'installation) <br>
```mysql> use civicrm; (pour travailler sur la base civicrm)``` <br>
```mysql> source sauvegarde_base_civicrm.sql``` <br>
```mysql> exit``` <br>
* Activer CiviCRM : **Wordpress > Extensions > CiviCRM** : activer,
* Reconstruire la base et les menus : <br>
Dans le menu Wordpress (bandeau de gauche) 
    * **Civicrm > Settings > Rebuild the CiviCRM menu**
    * **Civicrm > Settings > Rebuild the CiviCRM database triggers**.

## Migration des documents (pdf, notes) associés aux contacts
Ces documents ne sont pas dans la base de données qui ne contient que leur chemin.

Ils sont dans ```/var/www/html/ddctest/wp-content/uploads/civicrm/custom/```

Il est impératif de copier ces fichiers du serveur source vers le serveur cible.

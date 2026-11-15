# Mises à jour de CiviCRM
## Préparation de la mise à jour
* Dans Worpress dÉsactivez l'extension CiviCRM,
* Sauvegardez :

    * le site,
    * les bases de données civicrm et worpress,
    * Ne pas oublier : /var/www/html/don_corps/wp-content/uploads/civicrm/custom/,
    * Dans les anciennes installations, le répertoire extensions qui devra être restauré en fin d'installation : */var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext*.<br>
    ```cp /var/www/html/don_corps/wp-content/pluggins/civicrm/civicrm/ext Backup_loin_du_site```.
    
    >Cette étape est inutile pour les installations récentes dans lesquelles les extensions sont dans */var/www/html/don_corps/wp-content/uploads/civicrm/ext* <br>
    Si votre installation est ancienne, voyez [Localisation des extensions](../../GuideAdmin/civicrm/extensions.md#localisation-des-extensions) pour les migrer dans leur localisation officielle actuelle. <br>

* Dans les installations récentes, le fichier *civicrm.settings.php* contient une ligne définissant la variable php `auto_detect_line_endings`. <br>
Il est obsolète et la ligne doit être supprimée du fichier.

    ```nano /var/www/html/ddctest/wp-content/uploads/civicrm/civicrm.settings.php``` <br>

    Recherchez *auto_detect_line_endings* et commentez les lignes

    ```// force PHP to auto-detect Mac line endings``` <br>
    ```//if (version_compare(PHP_VERSION, '8.1') < 0) {``` <br>
    ```// ini_set('auto_detect_line_endings', '1');``` <br>
    ```//}``` <br>

* Flushez l'installation <br>
```cd /var/www/html/don_corps/wp-content/``` <br>
```cv flush```

## Effacez les anciens fichiers
Effacer les fichiers existants pour CiviCRM et son cache :

```rm -rf /var/www/html/ddctest/wp-content/plugins/civicrm``` <br>
```rm -rf /var/www/html/ddctest/wp-content/uploads/civicrm/templates_c/*``` <br>

> ATTENTION: ne pas effacer */var/www/html/ddctest/wp-content/uploads/civicrm/custom* qui contient les documents liés aux contacts.

## Installez les nouveaux fichiers 
* Procédez comme à [installation](installcivi.md),
* Changez les permissions de CiviCRM : 
```chown -R www-data:www-data /var/www/html/ddctest/wp-content/plugins/civicrm```.

## Si votre site est ancien : Restaurez les extensions 
```cp Backup_loin_du_site /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/```

## Lancer l'installeur
* Dans Wordpress, activez l'extension CiviCRM,
* Allez sur : <br>[http://example.org/wp-admin/admin.php?page=CiviCRM&q=civicrm/upgrade&reset=1](http://example.org/wp-admin/admin.php?page=CiviCRM&q=civicrm/upgrade&reset=1),
* Acceptez la mise à niveau de votre base de donnéeS ; les nouveaux fichiers sont installés et la base upgradée.
* Rechargez la page d'accueil de CiviCRM,
* Si la mise à jour ne se lance pas, dans le bandeau de gauche de Wordpress, sous CiviCRM : **Settings > Upgrade CiviCRM**.
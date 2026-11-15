# Mises à jour de CiviCRM

> Les commandes ci-dessous utilisent le chemin `/var/www/html/ddctest`. Remplacez-le par celui de votre site.

## 1. Préparer la mise à jour

1. Videz les caches de CiviCRM :
    ```bash
    cd /var/www/html/ddctest
    cv flush
    ```

2. Dans WordPress, désactivez l'extension CiviCRM.

3. Sauvegardez :

    * les fichiers du site ;
    * les bases de données CiviCRM et WordPress ;
    * le dossier `/var/www/html/ddctest/wp-content/uploads/civicrm/custom/`, qui contient les documents liés aux contacts ;
    * **dans les anciennes installations uniquement**, vos extensions placées dans `/var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/` :
        ```bash
        mkdir -p ~/sauvegarde_civicrm
        cp -R /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext ~/sauvegarde_civicrm/
        ```

        > Cette étape est inutile pour les installations récentes, dans lesquelles les extensions sont dans `/var/www/html/ddctest/wp-content/uploads/civicrm/ext/`. <br>
        > Si votre installation est ancienne, voyez [Localisation des extensions](../../GuideAdmin/civicrm/extensions.md#localisation-des-extensions) pour les déplacer à leur emplacement actuel : c'est préférable à une sauvegarde et une restauration à chaque mise à jour.

4. **Dans les anciennes installations**, le fichier `civicrm.settings.php` contient un bloc qui définit l'option PHP `auto_detect_line_endings`, aujourd'hui obsolète. Ouvrez le fichier :
    ```bash
    nano /var/www/html/ddctest/wp-content/uploads/civicrm/civicrm.settings.php
    ```
    Recherchez `auto_detect_line_endings` et commentez le bloc :
    ```php
    // force PHP to auto-detect Mac line endings
    //if (version_compare(PHP_VERSION, '8.1') < 0) {
    //  ini_set('auto_detect_line_endings', '1');
    //}
    ```

## 2. Effacer les anciens fichiers

Effacez les fichiers de CiviCRM et son cache de modèles :

```bash
rm -rf /var/www/html/ddctest/wp-content/plugins/civicrm
rm -rf /var/www/html/ddctest/wp-content/uploads/civicrm/templates_c/*
```

> **Attention** : n'effacez pas `/var/www/html/ddctest/wp-content/uploads/civicrm/custom/`, qui contient les documents liés aux contacts, ni `/var/www/html/ddctest/wp-content/uploads/civicrm/ext/`, qui contient vos extensions.

## 3. Installer les nouveaux fichiers

1. Procédez comme pour l'[installation](installcivi.md).
2. Rétablissez les droits sur les fichiers de CiviCRM :
    ```bash
    chown -R www-data:www-data /var/www/html/ddctest/wp-content/plugins/civicrm
    ```

## 4. Anciennes installations uniquement : restaurer vos extensions

Ne restaurez **que les extensions que vous avez ajoutées**, une par une. Le dossier `ext` fourni avec CiviCRM contient aussi les extensions du cœur, dans leur nouvelle version : il ne faut pas les remplacer par les anciennes.

```bash
cp -R ~/sauvegarde_civicrm/ext/don_corps /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/
cp -R ~/sauvegarde_civicrm/ext/org.civicoop.civirules /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/
# … une ligne par extension ajoutée
```

## 5. Lancer la mise à niveau

1. Dans WordPress, activez l'extension CiviCRM.
2. Lancez la mise à niveau de la base de données en ligne de commande :
    ```bash
    cd /var/www/html/ddctest
    cv upgrade:db
    cv flush
    ```

    > Il est aussi possible de la lancer depuis le navigateur, à l'adresse `https://example.org/wp-admin/admin.php?page=CiviCRM&q=civicrm/upgrade&reset=1`, ou dans WordPress par **CiviCRM > Settings > Upgrade CiviCRM**. <br>
    > Mais avec une base volumineuse, cette étape peut dépasser le délai d'exécution de PHP (30 s par défaut) et s'interrompre. La ligne de commande n'a pas cette limite.

3. Vérifiez que la base est à jour et que les extensions sont bien installées :
    ```bash
    cv ext:list -L
    ```
    Aucune extension ne doit apparaître en `installed-missing`. Si CiviCRM signale des mises à niveau d'extensions en attente, relancez `cv upgrade:db`.

4. Rechargez la page d'accueil de CiviCRM.
# Trucs et astuces
## Réinitialiser la numérotation des contacts (ID)
CiviCRM utilise un contact ID qui s'incrémente à chaque entrée. <br>
Si vous effacez définitivement tous les contacts d'une base, l'incrémentation va continuer. Pour éviter cela :

* Supprimez tous les contacts :

    * Pour cela il faut supprimer tous les évènements, encaissements adhésions: faire une recherche et tous les sélectionner pour suppression
    * Puis supprimer les contacts : afficher la liste, tout sélectionner, action : *supprimer définitivement*
    * Supprimer ceux qui seraient en corbeille : recherche avancée, cocher chercher dans la corbeille, supprimer définitivement les résultats
    * Tous les contacts vont être supprimés sauf l'utilisateur connecté et l'organisation principale

* Allez dans PhpMyadmin, sur la table civicrm_contacts, onglet SQL, et lancez : <br>
```ALTER TABLE civicrm_contact AUTO_INCREMENT = 1```

## Changer le timeout de Wordpress
Lors de l'import de grosses bases, WP interrompt le processus en raison d'un timeout trop court (30 sec par défaut). Ce paramètre peut être changé dans php.ini

* Localiser php.ini <br>
```php -i | grep php.ini```

* Dans ce fichier, modifier la ligne :
```max_execution_time=30;``` <br>
    en remplaçant 30 par 0 pour désactiver cette limite

* Une fois l'import terminé, N'OUBLIEZ PAS DE REMETTRE LA VALEUR INITIALE

## Changer les limites de mémoire de phpMyAdmin
Lors de l'import de grosses bases, phpMyAdmin interrompt le processus en raison d'une taille trop importante. <br> 
Ce paramètre peut être changé dans php.ini :

* Localiser php.ini <br>
```php -i | grep php.ini```

* Dans ce fichier modifier 3 entrées : <br>
```memory_limit``` <br>
```post_max_size``` <br>
```upload_max_filesize``` <br>
Les valeurs entrées doivent être telles que <br>
memory_limit > post_max_size > upload_max_filesize

* Relancez le serveur Apache

## Créer une archive zip sans fichiers cachés mac
Lors de la création d'archives zip avec l'utilitaire mac, des fichiers (.DS_Store) et répertoires (.__MACOSX/) cachés sont créés. 

Pour l'éviter:

* utilisez zip au lieu de l'utilitaire Mac, cela évitera la création des répertoires .__MACOSX/
* excluez les fichiers .DS_Store : <br>
```zip -r civicrm-X.XX.X-wordpress_FR_API_ext.zip civicrm/ -x '**/.DS_Store'```

# Paramétrage des purges
L'article 4 de l'[arrêté du 22 juillet 2023](https://www.legifrance.gouv.fr/download/pdf?id=i18Wkuyn4Vl70eYdM8iDKjj6UFbgHwXsc1xpBHveUmo=) précise les conditions de conservation des données liées aux donneurs, aux proches et aux utilisateurs du centre d'accueil des corps. 

La purge des données comporte plusieurs étapes :

* Des requêtes SearchKit rattachent les contacts à des groupes dynamiques (par exemple *donneurs dont les opérations funéraires sont achevées depuis plus de 5 ans*, *Dons annulés*...),
* une API purge les contacts de chacun des groupes en appliquant des règles différentes, définies par l'arrêté : <br>```/var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/don_corps/api/v3/Contact/Purge.php```

> Pour que l'API fonctionne, vous devez avoir installé [cv](../../GuideAdmin/civicrm/cv.md) préalablement.

## Définir le fichier de log des purges (optionnel): <br>

Les logs des purges s'écrivent par défaut dans **/var/www/html/ddctest/wp-content/uploads/civicrm/ConfigAndLog.purge.log**. Si vous souhaitez modifier la localisation de ce fichier :
```nano /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/don_corps/api/v3/Contact/Purge.php``` <br>

Modifiez à la valeur souhaitée : la variable ```$logfile ```

## Lancez les purges à la main 
```cd /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/don_corps/```<br>
```cv api Contact.Purge``` <br>
Les groupes de purges sont traités et les contacts sont supprimés ou leur identité est modifiée pour *Anonymisé ANONYMISE*.

## Automatiser les purges 
Une tâche Call Contact.Purge peut lancer la purge automatiquement chaque jour : <br>
**Administrer > Paramètres Système > Tache programmées**

Elle est activée par défaut. 

Pour la désactiver, allez dans la ligne de cette tâche sur **plus > Désctiver**


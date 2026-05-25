# Journalisation normale et étendue
## Journalisation simple
Par défaut CiviCRM journalise de façon sommaire des actions réalisées sur les contacts. <br>
Pour les visualiser aller sur la fiche du contact > *Onglet Journal des modifications* : 

![Journalisation simple](../../img/journal1.png)

Cette commande affiche qui est intervenu sur le dossier et à quelle date mais pas le type d'action.
## Journalisation étendue
Il est possible de journaliser à quel moment qui a fait quoi. Le même onglet affiche alors une vue plus détaillée des actions réalisées. <br>
Si vous cliquez sur une des actions vous pouvez voir le détail de cette action et il est possible de revenir à l'état antérieur en cliquant sur *Rétablir ces changements*

![Journalisation étendue](../../img/journal2.png)

L'arrêté du 11 juillet 2023 précise que **La procédure de gestion des informations doit permettre une traçabilité complète sur toutes les opérations effectuées en terme de saisie et de consultation*<br>
Il est donc fortement recommandé d'activer la journalisation étendue.

# Comment activer la journalisation étendue ?
## Installer l'extension *Advanced logging with Changelog*
* la télécharger 
[lab.civicrm.org](https://lab.civicrm.org/extensions/advloggingwithchangelog/-/archive/master/advloggingwithchangelog-master.zip?ref_type=heads)
* copier l'archive et dézippez là dans le dossier : <br>
```/var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/```<br>
* Allez dans Administrer > Paramètres Système > Extensions > Actualiser
* L'extension apparait dans la liste
* Cliquez sur installer
## Créer une base de donnée dédiée
Cette étape n'est pas obligatoire ; par défaut CivicRM crée une copie de toutes les tables avec le préfixe log_ dans la base civicrm. Il est conseillé de créer une nouvelle base, civicrm_logging, avec les meme privilèges que la base civicrm, pour recevoir les logs.<br>

Une fois la base de journalisation crée : 
``` nano */var/www/html/ddc/wp-content/uploads/civicrm/civicrm.settings.php```<br>

    Remplacer : 
    define(
      'CIVICRM_LOGGING_DSN' ,
       'mysql://user:pass@localhost/db_name?new_link=true'
        );
    par 


    define(
      'CIVICRM_LOGGING_DSN' ,
       'mysql://civicrm:<MDPCIVICRM>@localhost/civicrm?new_link=true'
        );

## Activez la journalisation étendue
Allez dans *Administrer > Paramètres système > Divers*<br>
Journalisation > activer

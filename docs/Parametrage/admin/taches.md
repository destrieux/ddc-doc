# Gestion des tâches périodiques
## Introduction
CiviCRM gère des taches périodiques avec une fréquence définie dans :<br>
**Administrer > Paramètres Systèmes > Travaux programmés**.

Ces tâches sont exécutées via une API, *job.execute*, par un utilisateur *crontab* (administrateur de Wordpress) pour interagir avec CiviCRM via wp-cli, le langage de communication avec Wordpress. <br>

Cette metatâche programmée, *job.execute*, est elle-même lancée par www-data via cron à une fréquence choisie par l'utilisateur.

Pour plus d'infos : [https://docs.civicrm.org/sysadmin/en/latest/setup/jobs/](https://docs.civicrm.org/sysadmin/en/latest/setup/jobs/)

## Installer wp-cli 
**wp-cli** est le langage de communication de CiviCRM avec Wordpress. Il est utilisé pour lancer des tâches programmées.

Son installation est détaillée [ici](../../GuideAdmin/civicrm/cv.md)

## Tester le lancement manuel de l'API job.execute 
Tester l'exécution de *job.execute* par l'utilisateur *www-data* et l'utilisateur *cronuser* de Wordpress
```sudo -u www-data /usr/local/bin/wp --user=cronuser --url=http://SITE --path=/var/www/html/ddctest civicrm api job.execute auth=0```

Remplacer :

* SITE par l'adresse de votre site,
* /var/www/html/ddctest par le chemin ou sont situés les fichiers de votre site.

Normalement les taches s’exécutent et sont listées dans les journaux accessibles dans :
**Administrer > Paramètres Systèmes > Travaux programmés**.

## Régler cron
Cron définit la périodicité d'exécution de job.execute.

Par exemple, pour lancer job.execute toutes les 5 minutes:

* éditer crontab
```crontab -e -u www-data``` <br>

* ajouter la ligne suivante <br>
```*/5 * * * * /usr/local/bin/wp --user=cronuser --url=http://ddc-test --path=/var/www/html/ddctest civicrm api job.execute auth=0```

Remplacer ```/usr/local/bin/wp --user=cronuser --url=http://SITE --path=/var/www/html/ddctest civicrm api job.execute auth=0``` <br>par la ligne testée plus haut (remplacer CHEMIN et SITE)

## Définir les tâches à réaliser périodiquement
Dans **Administrer > Paramètres Systèmes > Travaux programmés,** <br>
choisir les taches à faire tourner automatiquement, par exemple :

* CiviCRM update check (quotidienne),
* Clean-up temporary Data and files (horaire),
* Send Scheduled Mailings (Always, c'est à dire à chaque fois que job.execute est lancé, à la fréquence définie dans crontab)

Vous pouvez vérifier le bon déroulement des tâches programmées dans les journaux accessibles dans 
**Administrer > Paramètres Système > Travaux programmés**
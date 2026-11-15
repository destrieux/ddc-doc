# Extensions requises

L'extension don_corps nécessite le téléchargement préalable des extensions suivantes :

| Extension | Rôle |
|-----------|------|
| [de.systopia.civioffice](https://github.com/systopia/de.systopia.civioffice/releases) | Génère des documents Word et PDF à partir de modèles Word |
| [org.civicoop.civirules](https://lab.civicrm.org/extensions/civirules/-/archive/master/civirules-master.zip?ref_type=heads) | Déclenche des actions lorsque certaines conditions sont réunies |
| [org.civicoop.emailapi](https://lab.civicrm.org/extensions/emailapi/-/archive/master/emailapi-master.zip?ref_type=heads) | Envoie des courriels lorsque certaines règles sont vérifiées |
| [org.civicoop.civiruleslogger](https://github.com/CiviCooP/org.civicoop.civiruleslogger/archive/refs/heads/master.zip) | Journalise les actions déclenchées par les règles |
| [org.civicoop.documents](https://lab.civicrm.org/extensions/documents/-/archive/master/documents-master.zip?ref_type=heads) | Gère les documents téléchargés (mini GED) |
| org.civicrm.afform_admin | Génère les formulaires de recherche à partir de requêtes enregistrées <br> (déjà fournie avec le cœur de CiviCRM) |
| [org.civicrm.contactlayout](https://lab.civicrm.org/extensions/contactlayout/-/archive/master/contactlayout-master.zip?ref_type=heads) | Gère les grilles d'affichage des contacts |
| [de.systopia.eventmessages](https://github.com/systopia/de.systopia.eventmessages/archive/refs/heads/master.zip) | Envoie les courriels relatifs aux événements (cérémonies…) |
| [sktokens](https://lab.civicrm.org/extensions/sktokens/-/archive/main/sktokens-main.zip?ref_type=heads) | Crée des tokens à partir de la base de données, utilisables dans les messages |
| [Advanced logging with Changelog](https://lab.civicrm.org/extensions/advloggingwithchangelog/-/archive/master/advloggingwithchangelog-master.zip?ref_type=heads) | Combine les journalisations simples (qui a fait) et détaillées (quoi et quand) |

> Pour le moment, l'extension [FrenchCodesPostaux](https://github.com/allinappli/frenchCodePost-civicrm), qui utilise l'API de La Poste pour compléter les adresses, est incompatible avec org.civicrm.contactlayout.

## Localisation des extensions

* Historiquement, les extensions ajoutées par l'utilisateur étaient placées dans le dossier `/var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/`, qui est écrasé à chaque mise à jour de CiviCRM, avec un risque de les perdre.
* Elles sont maintenant dans `/var/www/html/ddctest/wp-content/uploads/civicrm/ext/`, qui n'est pas modifié par les mises à jour de CiviCRM.
* Pour déplacer vos extensions (anciennes installations) :

    1. Placez-vous à la racine de votre site, par exemple :
        ```bash
        cd /var/www/html/ddctest/
        ```
    2. Vérifiez le répertoire où CiviCRM cherche les extensions additionnelles :
        ```bash
        cv vget extensionsDir
        ```
        Vous devez obtenir `[civicrm.files]/ext/`, c'est-à-dire le répertoire `/var/www/html/ddctest/wp-content/uploads/civicrm/ext/`.
    3. Déplacez les extensions que vous avez ajoutées (une seule copie doit être présente dans le système) :
        ```bash
        cd /var/www/html/ddctest/wp-content/plugins/civicrm/civicrm/ext/
        mv advloggingwithchangelog      /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv de.systopia.civioffice       /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv de.systopia.eventmessages    /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv don_corps                    /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv org.civicoop.civirules       /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv org.civicoop.civiruleslogger /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv org.civicoop.documents       /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv org.civicoop.emailapi        /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv org.civicrm.contactlayout    /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        mv sktokens                     /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
        ```
    4. Rafraîchissez les caches pour que CiviCRM localise à nouveau les extensions :
        ```bash
        cv flush
        ```
    5. Vérifiez que vos extensions sont bien installées (aucune ne doit apparaître en `installed-missing`) :
        ```bash
        cv ext:list -L
        ```

## Téléchargement

* Vous pouvez télécharger la dernière version de chaque extension en suivant les liens du tableau ci-dessus. Chaque extension doit être téléchargée et décompressée dans le dossier `/var/www/html/ddctest/wp-content/uploads/civicrm/ext/`.
* Vous pouvez aussi utiliser une archive contenant toutes les extensions nécessaires. Elle doit être téléchargée et décompressée dans ce même dossier.

> Il n'est pas nécessaire d'installer les extensions à ce stade : il suffit de les télécharger et de les déposer dans le dossier *ad hoc*. Elles s'installeront en même temps que l'extension *don_corps*.

## Contournement du bug CiviOffice

* Il existe un bug dans les versions récentes de CiviOffice : l'impression d'un document à partir d'un événement, ou de toute autre entité sans contact associé, est bloquée. Ce problème [a été signalé sur GitHub](https://github.com/destrieux/don_corps/issues/3).
* Pour le contourner, il faut modifier une ligne dans le code de CiviOffice :
    ```bash
    cd /var/www/html/ddctest/wp-content/uploads/civicrm/ext/de.systopia.civioffice/CRM/Civioffice
    cp ConversionJob.php ConversionJob.php.bak
    nano ConversionJob.php
    ```
    Dans la méthode `run()`, remplacez la ligne :
    ```php
    'target_id' => [$contact_id],
    ```
    par :
    ```php
    'target_id' => !empty($contact_id) ? [$contact_id] : [],
    ```

> **Ce contournement sera écrasé à chaque mise à jour de CiviOffice tant que le bug ne sera pas corrigé : il devra alors être réappliqué.**
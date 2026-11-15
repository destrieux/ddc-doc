# Installation de l'extension don_corps

> Avant de commencer, téléchargez les extensions nécessaires : voir [Extensions requises](extensions.md).

1. Téléchargez l'extension [don_corps](https://github.com/destrieux/don_corps/archive/refs/heads/main.zip).

2. En tant que superutilisateur (root), décompressez-la dans le dossier des extensions, puis **renommez le dossier** `don_corps-main` en `don_corps` :
    ```bash
    cd /var/www/html/ddctest/wp-content/uploads/civicrm/ext/
    unzip ~/don_corps-main.zip
    mv don_corps-main don_corps
    chown -R www-data:www-data don_corps
    ```

    > Le dossier doit porter exactement le nom de l'extension (`don_corps`), sinon CiviCRM ne la trouve pas.

3. Rafraîchissez CiviCRM pour qu'il détecte la nouvelle extension :
    ```bash
    cd /var/www/html/ddctest
    cv flush
    ```

4. Dans **un second terminal**, suivez le journal de CiviCRM pendant l'installation. Les messages d'installation y sont écrits, alors que le terminal en affiche peu :
    ```bash
    cd /var/www/html/ddctest/wp-content/uploads/civicrm/ConfigAndLog
    tail -f CiviCRM.*.log
    ```
    **Ctrl+C** arrête le suivi une fois l'installation terminée.

5. Dans le premier terminal, lancez l'installation :
    ```bash
    cd /var/www/html/ddctest
    cv ext:enable don_corps
    ```
    Pour un affichage détaillé : `cv -vvv ext:enable don_corps`.

> N'installez pas l'extension depuis l'interface web : l'installation est longue (don_corps et les extensions dont elle dépend) et échouerait en dépassant le délai d'exécution de PHP.
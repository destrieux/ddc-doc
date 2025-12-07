# Installer unoconv, les codes-barres, cv et cli
## Unoconv
Unoconv est un convertisseur qui génère des pdf à partir de documents Word; il est utilisé par l'extension CiviOffice qui génère les documents depuis la base. <br>
### Si vous utilisez une version debian <13 (Trixie)
Unoconv est disponible sous forme de paquet et installe LibreOffice.

    apt-get update 
    apt-get install unoconv 

### Si vous utilisez une version debian > 12 (Bookworm)
Unoconv n'est plus maintenu dans Trixie (13) et va etre remplacé par unoserver, pas encore utilisé par CiviOffice. Vous devez donc l'installer séparément.  <br>

Il a besoin de LibreOffice.  <br>

    apt-get update 
    apt-get install libreoffice 

Il a aussi besoin de du connecteur python UNO qui s'installe normalement avec le paquet LibreOffice.  <br>

Pour rechercher si les librairies sont bien installées : 

    cd /tmp
    wget https://gist.githubusercontent.com/regebro/036da022dc7d5241a0ee97efdf1458eb/raw/1bc0655423d196acd79a5d9fa60d2baada8dd534/find_uno.py
    python3 find_uno.py

Retourne la liste des installations python avec les librairies LibreOffice installées.

Pour installer unoconv dans /usr/bin

    cd /usr/bin
    wget https://raw.githubusercontent.com/unoconv/unoconv/refs/heads/master/unoconv
    sudo chmod ugo+x unoconv

Modifier le unoconv qui est un script python pour ajouter la version de python à utiliser et remplacer la commande *distutils.version* (obsolète) par *packaging.version*.

    nano /usr/bin/unoconv

Remplacer la ligne 

    #!/usr/bin/env python
    par 
    #!/usr/bin/env python3 (la version retournée plus haut par find_uno.py)

et

    from distutils.version import LooseVersion
    par 
    from packaging.version import Version as LooseVersion

### Pour tester, 
Tapez ```unoconv``` qui retourne le message suivant : <br>
```unoconv: you have to provide a filename or url as argument```<br>
```Try unoconv -h for more information.``` <br>

## Police LibreBarcode39-Regular.ttf 
Elle sera utile pour imprimer des codes-barres. <br>
Elle est téléchargeable depuis [Google Fonts](https://fonts.google.com/specimen/Libre+Barcode+39).

```cp LibreBarcode39-Regular.ttf /usr/local/share/fonts/``` <br>
```chmod 644 /usr/local/share/fonts/LibreBarcode39-Regular.ttf``` <br>
```fc-list | grep LibreBarcode39``` <br>

La dernière commande vérifie que la police est bien disponible ; elle doit retourner son chemin.

## cv
cv est une interface en ligne de commandes pour interagir avec CiviCRM. <br>
Son installation est indispensable à l'utilisation des API et à l'installation en ligne de commandes.

Vérifiez que cv n'est pas installé en tapant ```cv``` dans un terminal :

### Si cv n'est pas installé, 

    sudo curl -LsS https://download.civicrm.org/cv/cv.phar -o /usr/local/bin/cv
    sudo chmod ug+x /usr/local/bin/cv

### Si cv est installé, une aide va s'afficher

## Installer wp-cli
wp-cli est le langage de communication de CiviCRM avec Wordpress. Il est utilisé pour lancer des tâches programmées.

    sudo curl -LsS https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar -o /usr/local/bin/wp
    sudo chmod ug+x /usr/local/bin/wp

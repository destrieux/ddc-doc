# Installation de l'extension don_corps
* Téléchargez l'extension [don_corps](https://github.com/destrieux/don_corps/archive/refs/heads/main.zip) <br>
En su, déplacez là et décompactez là dans : <br>
```/var/www/html/ddctest/wp-content/uploads/civicrm/ext/```<br>
```chown -R www-data:www-data /var/www/html/ddctest/wp-content/uploads/civicrm/ext/don_corps```
* Rafraîchissez CiviCRM<br>
```cd /var/www/html/ddctest/wp-content/uploads/civicrm/ext/don_corps```<br>
```cv flush``` <br>
* Lancez l'installation<br>
```cv ext:enable don_corps``` <br>
(en mode verbose : cv -vvv ext:enable don_corps)<br>

> Ne pas installer en mode graphique car l'installation est longue (installation de don_corps et des autres extensions nécessaire) et échouerait en raison d'un time-out.
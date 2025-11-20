# Paramètres de messagerie

## Adresses d'expédition
Vous pouvez définir ici plusieurs adresses d'expédition (secrétarait, techniciens...).<br>
Il est conseillé à l'administrateur de créer des adresses fonctionnelles de messagerie et de les inscrire ici.

Allez à **Administrer > Communication > Site From email adresses**, <br>
**Ajouter une adresse de courriel**.

* **Nom affiché** : celui qui s'affichera lors du choix de l'expéditeur et dans les courriels,
* **Courriel** : adresse courriel,
* **Description** : texte libre,
* **Par défaut** : à cocher pour l'adresse que vous utiliserez par défaut.

## SMTP/Sendmail
Vous devez définir ici comment les courriels seront envoyés.<br>
**Administrer > Paramètres Système > Courriers sortants (SMTP/Sendmail)**<br>
Modifiez les champs suivants si besoin : 

* **Simple mail limit** : c'est le nombre maximal de courriels à expédier en une fois. Ce nombre ne doit pas être trop élevé afin que votre site ne soit pas considéré comme envoyant du spam. <br>
Au-dessus de ce nombre, les messages sont envoyés par paquets.
* **Configuration Mailer/SMTP** : à définir par l'administrateur

Il est possible d'envoyer un courriel de test pour vérifier le paramétrage : <br> **Enregistrer et envoyer un courriel de test**

## Courrier sortant et import de base
Lors de l'[import d'une base existante](../../GuideAdmin/civicrm/migration.md), celle-ci contient des valeurs de serveur et notamment des infos sur les clés utilisées. <br>
Ces infos peuvent être discordantes avec celles contenues dans civicrm.settings.php. <br>
Lorsque vous tentez de paramétrer le serveur de courrier vous obtenez : <br>
```Failed to find key by ID or tag (dQ4Av82utXEH28r_ebThCb2L8Y8)```

Pour régler le problème :

* dans PhpMyadmin, base *civicrm*, ouvrir la table *civcrm_setting* <br>
    recherchez l'entrée mailing_backend <br>
    effacez sa valeur <br>
    redéfinissez les paramètres smtp dans les menus de CiviCrm : <br>
    **Administrer > Paramètres Système > Courrier sortant**.


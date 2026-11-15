# Lister les activités d'enseignement et de recherche et générer les devis

## Pour afficher la liste

Accédez à la liste : **Événements > Activités au laboratoire > Recherche Activité** <br>
La liste des activités s'affiche ; vous pouvez filtrer :

* sur l'**intitulé**, le **N°**, le **porteur** ou le **déposant** de saisine. La recherche se fait sur ces différents champs à la fois ;
* sur les **dates de début et de fin**.

Dans le tableau :

* si le **N° de saisine** est absent, le **Titre** de l'activité est en rouge ;
* vous pouvez cliquer sur le **Titre d'activité** ou le **N° de saisine** pour accéder aux fiches correspondantes et les compléter ;
* vous pouvez modifier certains paramètres directement dans le tableau :
    * **Confirmation** : le passage à *Oui* « dégrise » la ligne ;
    * **Date de début**, **Date de fin** ;
    * **Type d'activité**.

## Pour éditer un devis

* Cochez la case de l'activité.
* Choisissez **Action > Create Document (CiviOffice)**.
* Choisissez le modèle de devis et le format (PDF, DOCX…).
* Dans **Create activity**, choisissez **Devis** (le type d'activité a peu d'importance, mais ne le laissez pas vide).

Puis cliquez sur **Create Document**.

Le document est téléchargé, mais n'est pas enregistré dans CiviCRM.

> Il existe un bug dans CiviOffice qui bloque l'impression (rien ne se passe après avoir cliqué sur **Create Document**). <br>
> Il a été signalé, mais en attendant, [votre administrateur serveur doit faire une petite modification à chaque mise à jour de CiviOffice](../../GuideAdmin/civicrm/extensions.md#contournement-bug-civioffice).
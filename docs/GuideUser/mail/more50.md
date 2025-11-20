# Envois à plus de 50 contacts
La procédure est différente afin que les messages envoyés ne soient pas considérés comme du spam. Le gestionnaire des tâches de CiviCRM va envoyer les courriels par paquets de 50. 

Il faut créer un groupe de contacts puis envoyer le message aux membres de ce groupe.
## Créer un groupe dynamique

Allez à **Recherche > Search Kit** pour créer une recherche personnalisée.<br>
[La documentation de Search Kit se trouve ici.](https://docs.civicrm.org/user/en/latest/searching/searchkit/what-is-searchkit/)

Créez un groupe dynamique à partir de cette recherche : le groupe contiendra tous les contacts qui satisfont la recherche ; <br>
il se mettra à jour en cas de modification de la recherche ou des critères des contacts.

Les groupes disponibles sont visibles ici : **Contacts > Groupes > Gestion des groupes**.

## Préparer le message
Allez dans **Mailings > Nouveau Mailing**

Dans Destinataires, choisissez le groupe que vous venez de créer.<br>
Composez votre message et envoyez le.
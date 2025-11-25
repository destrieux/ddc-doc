# Suppression de pièces en lot
Il est possible de supprimer de nombreuses pièces en une seule opération, par la création d'une activité de type *Suppression d'un lot de pièces*.
> Cete fonction est sans action sur les corps pour lesquels la [procédure de demande de transport et crémation](../donneur/funerarium/prepadepart.md ) doit être suivie.

* Depuis une fiche de contact quelconque (donneur, personnel, centre de don...),
* **Action > Suppression d'un lot de pièces**,
* Entrez la liste de pièces dans le champ *Détails*, **Enregistrer**.

Pour chacune des pièces :
 
 * les champs *localisation* et *complément de localisation* sont vidés,
 * le *mode d'élimination* passe à *Crémation/Opérations funéraires*.

Si une des pièces avait été préalablement détruite ou qu'elle n'existe pas dans la base, une alerte est produite.

Dans tous les cas un log des suppressions est inscrit dans le champ *Détails* de l'utilisation.
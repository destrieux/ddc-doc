# Identifiant numérique et Utilisations du corps
## Identifiant numérique
Un identifiant numérique unique est attribué à chaque pièce ou corps. 
Il est inscrit sous forme de code-barres sur une étiquette ou un bracelet attaché à la pièce ou au corps.

> Il est recommandé de se procurer un lecteur de codes-barres et des jeux de bracelets et d'étiquettes codes-barres en prenant soins de ne pas réutiliser les numéros : chaque étiquette / bracelet doit être unique. <br>
> Nous vous suggérons de commander des bracelets de couleurs différentes pour identifier facilement les corps pour lesquels une restitution était souhaitée par le donneur

## Utilisations du corps
Cet onglet de la fiche Donneur répertorie toutes les utilisations du corps ou des pièces qui en proviennent.<br>

A l'arrivée du donneur, une première **Utilisation** est créée pour le corps. Un premier code-barres lui est attribué sous forme d'un bracelet attaché à son poignet ou à sa cheville.

> Bien vérifier dès ce stade les souhaits funéraires du donneur pour identifier par un bracelet de couleur différente ceux pour lesquels une restitution serait envisageable.

Si un organe est prélevé, une autre utilisation, avec un autre code-barres lui est ajoutée. 
> Il doit donc y avoir une **Utilisation** - et donc un code-barres - pour le corps et une pour chaque organe, pièces anatomique ou prélèvement provenant de ce donneur. <br> La seule façon de relier les prélèvements, et le corps est d'interroger la base.

Pour créer une utilisation du corps, dans l'onglet **Utilisation du corps** de la fiche du donneur, cliquez sur **Add Utilisation du Corps** et remplissez les champs qui s'affichent :

### N° de pièce
C'est le numéro identifiant unique indiqué sur le bracelet du corps ou sur les étiquettes des contenants de pièces anatomiques. <br>
Un lecteur de codes-barres connecté à votre PC permet la saisie des numéros sans erreur.

### Type de pièce et Côté
Il peut s'agir d'un corps, d'un organe ou d'un segment de corps. Plusieurs options peuvent être sélectionnées. <br>
La liste est modifiable par votre administrateur en cliquant sur la clé à molette en bout de ligne.

### Préparé par
Ce champ recherche un membre du personnel dans la base de données.
> Les personnels du centre d'accueil doivent avoir [préalablement été inscrits dans la base](../../../Parametrage/user.md).<br>
> Vous pouvez utiliser le caractère générique % pour remplacer n'importe quel caractère dans la recherche de contact.

### Utilisation [défaut : *En attente d'utilisation*]
Il s'agit de la catégorie d'utilisation, par exemple enseignement pour le C1, le C2, le C3, Recherche...<br>
Ce champ devra être complété au fur et à mesure de l'utilisation du corps ou de la pièce.
>La liste est modifiable par votre administrateur en cliquant sur la clé à molette en bout de ligne.

### Protocole de recherche ex vivo [défaut : *Pas de protocole*]
C'est l'intitulé d'un protocole de recherche *post mortem* éventuel dans lequel le donneur serait inscrit.
>La liste est modifiable par votre administrateur en cliquant sur la clé à molette en bout de ligne.

### Complément ou identifiant
Par exemple, intitulé du workshop ou identifiant de la pièce/corps dans un protocole de recherche. 

### Délai entre heure de décès et injection / prélèvement 
Entrez le délai entre l'heure de décès et l'injection ou le prélèvement.
>Attention bien mettre un *délai* et non une heure d'injection/prélèvement car la date et l'heure de décès seront purgées 5 ans après les opérations funéraires et vous perdrez ces données.

En l'absence de prélèvement (organe, segment, tissu...) ou d'injection  : -1, <br>
Si délai inconnu, notez -5.

### Site et médium injectés
Plusieurs options peuvent être sélectionnées.
>La liste est modifiable par votre administrateur en cliquant sur la clé à molette en bout de ligne.

### Imagerie, Inclusion en paraffine, Klingler
Plusieurs options peuvent être sélectionnées.
>Les listes sont modifiables par votre administrateur en cliquant sur la clé à molette en bout de ligne.

### Localisation : 
Choisir ici le lieu de conservation interne ou externe à votre centre dans lequel le corps ou la pièce se trouvent. 
> Ces lieux ont dû être crées plus haut, <br>
> Vous pouvez utiliser le caractère générique % pour remplacer n'importe quel caractère dans la recherche de contact.

La localisation devra être mise à jour tant que le corps, l'organe ou la pièce ne seront pas détruits.

### Complément localisation
Il s'agit d'un champ libre, par exemple le numéro de la case réfrigérée, celui de l'étagère où se situent le corps ou la pièce.

Ce champ devra être mis à jour tant que le corps, l'organe ou la pièce ne seront pas détruits.
### Date de prêt éventuel et date de Retour
A renseigner en cas de sortie de pièce ou de corps.
> Ne pas oublier de changer la **Localisation** du corps ou de la pièce en modifiant l'**Utilisation**

### Mode d'élimination  [défaut : *Non éliminée*]
> Attention si vous passez ce champ à demander crémation, un mail est envoyé automatiquement à votre prestaire funéraire ([cf. tableau de bord des corps présents](../../gestion_pieces/tableau.md))

### Date d'élimination
A remplir pour les pièces et non pour les corps, pour lesquels il existe une date d'opérations funéraires.

### Remarques
Champ libre

### Inventaire
Liste les inventaires dans lesquels la pièce est présente

## Rechercher un corps ou une pièce 
Allez à **Corps et pièces anatomiques > Recherche Code-Barres**

Le formulaire permet de filtrer par type de pièce, par protocole, par n° de pièce ou de corps, utilisation ou mode d'élimination.
---
id: propertiesSubform
title: Sous-formulaire
---

---
## Autoriser la suppression

Indique si l’utilisateur peut supprimer des sous-enregistrements dans un sous-formulaire liste.

#### Grammaire JSON

| Nom             | Type de données | Valeurs possibles               |
| --------------- | --------------- | ------------------------------- |
| deletableInList | boolean         | true, false (par défaut : true) |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)


---
## Formulaire détaillé

Cette option permet de désigner un formulaire détaillé à utiliser dans un sous-formulaire. Il peut être :

- un widget, c'est-à-dire un sous-formulaire de type page doté de fonctions spécifiques. Dans ce cas, [le sous-formulaire de liste](#list-form) et les propriétés [Source](#source) doivent être vides ou non présents.   
  Vous pouvez sélectionner un nom de formulaire de composant lorsqu'il est publié dans le composant.
> Vous pouvez générer des [composants](Concepts/components.md) fournissant des fonctionnalités supplémentaires via des sous-formulaires.

- le formulaire détaillé à associer au [sous-formulaire de liste](#list-form). The detail form can be used to enter or view subrecords. It generally contains more information than the list subform. Naturally, the detail form must belong to the same table as the subform. You normally use an Output form as the list form and an Input form as the detail form. If you do not specify the form to use for full page entry, 4D automatically uses the default Input format of the table.


#### Grammaire JSON

| Nom        | Type de données | Valeurs possibles                                                                                                                   |
| ---------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| detailForm | string          | Name (string) of table or project form, a POSIX path (string) to a .json file describing the form, or an object describing the form |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)

---
## Double-click on empty row

Action to perform in case of a double-click on an empty line of a list subform. Les options suivantes sont disponibles :
- Ne rien faire : ignore le double-clic.
- Ajouter un enregistrement : crée un nouvel enregistrement dans le sous-formulaire et passe en mode édition. L'enregistrement sera créé directement dans la liste si la propriété [Saisissable dans la liste] est activée. Sinon, il sera créé en mode page, dans le [formulaire détaillé](detail-form) associé au sous-formulaire.


#### Grammaire JSON

| Nom                          | Type de données | Valeurs possibles                  |
| ---------------------------- | --------------- | ---------------------------------- |
| doubleClickInEmptyAreaAction | string          | "addSubrecord" ou "" to do nothing |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)

#### Voir également
[Double click on row](#double-click-on-row)

---
## Double-clic sur ligne

`List subform`

Sets the action to be performed when a user double-clicks on a row in a list subform. The available options are:

*   **Do nothing** (default): Double-clicking a row does not trigger any automatic action.
*   **Edit Record**: Double-clicking a row displays the corresponding record in the [detail form defined for the list subform](#detail-form). The record is opened in read-write mode so it can be modified.
*   **Display Record**: Identical to the previous action, except that the record is opened in read-only mode so it cannot be modified.

Regardless of the action selected/chosen, the `On Double clicked` form event is generated.

For the last two actions, the On `Open Detail` form event is also generated. The `On Close Detail` is then generated when a record displayed in the detail form associated with the list box is about to be closed (regardless of whether or not the record was modified).

#### Grammaire JSON

| Nom                    | Type de données | Valeurs possibles                   |
| ---------------------- | --------------- | ----------------------------------- |
| doubleClickInRowAction | string          | "editSubrecord", "displaySubrecord" |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)


#### Voir également
[Double click on empty row](#double-click-on-empty-row)

---
## Enterable in list

Lorsque cette propriété est activée pour un sous-formulaire de liste, l'utilisateur peut modifier les données de l'enregistrement directement dans la liste, sans avoir à utiliser le [formulaire détaillé associé](#detail-form).

> Pour cela, il vous suffit de cliquer deux fois sur le champ à modifier afin de le passer en mode édition (veillez à laisser suffisamment de temps entre les deux clics pour ne pas générer de double-clic).


#### Grammaire JSON

| Nom             | Type de données | Valeurs possibles |
| --------------- | --------------- | ----------------- |
| enterableInList | boolean         | true, false       |


#### Objets pris en charge

[Sous-formulaire](subform_overview.md)


---
## List Form

Cette option permet de désigner un formulaire liste à utiliser dans un sous-formulaire. Un sous-formulaire en liste vous permet de saisir, visualiser et modifier des données dans d’autres tables.

Les sous-formulaires de liste peuvent être utilisés pour la saisie de données de deux manières : l'utilisateur peut saisir des données directement dans le sous-formulaire ou les saisir dans un [formulaire de saisie](#detail-form). Dans cette configuration, le formulaire utilisé comme sous-formulaire est appelé formulaire Liste. Le formulaire de saisie est appelé le formulaire détaillé.

#### Grammaire JSON

| Nom      | Type de données | Valeurs possibles                                                                                                                   |
| -------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| listForm | string          | Name (string) of table or project form, a POSIX path (string) to a .json file describing the form, or an object describing the form |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)



---
## Source

Spécifie la table à laquelle appartient le sous-formulaire Liste (le cas échéant).

#### Grammaire JSON

| Nom   | Type de données | Valeurs possibles                                     |
| ----- | --------------- | ----------------------------------------------------- |
| table | string          | Nom de la table 4D, ou "" s'il n'existe aucune table. |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)

---
## Mode de sélection

Designates the option for allowing users to select rows:
- **None**: Rows cannot be selected if this mode is chosen. Clicking on the list will have no effect unless the [Enterable in list](subform_overview.md#enterable-in-list) option is enabled. The navigation keys only cause the list to scroll; the `On Selection Change` form event is not generated.
- **Single**: One row at a time can be selected in this mode. Clicking on a row will select it. A **Ctrl+click** (Windows) or **Command+click** (macOS) on a row toggles its state (between selected or not).  
  The Up and Down arrow keys select the previous/next row in the list. The other navigation keys scroll the list. The `On Selection Change` form event is generated every time the current row is changed.
- **Multiple**: Several rows can be selected simultaneously in this mode.
    - The selected subrecords are returned by the `GET HIGHLIGHTED RECORDS` command.
    - Clicking on the record will select it, but it does not modify the current record.
    - A **Ctrl+click** (Windows) or **Command+click** (macOS) on a record toggles its state (between selected or not). The Up and Down arrow keys select the previous/next record in the list. The other navigation keys scroll the list. The `On Selection Change` form event is generated every time the selected record is changed.


#### Grammaire JSON

| Nom           | Type de données | Valeurs possibles            |
| ------------- | --------------- | ---------------------------- |
| selectionMode | string          | "multiple", "single", "none" |

#### Objets pris en charge

[Sous-formulaire](subform_overview.md)

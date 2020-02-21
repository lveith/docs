---
id: version-18.0-method
title: '$method'
original_id: method
---

Ce paramètre vous permet de définir l'opération à exécuter avec l'entité ou la sélection d'entité (entity selection) retournée.

## Syntaxe

| Syntaxe                                         | Exemple                                                                                         | Description                                                                                                          |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [**$method=delete**](#methoddelete)             | `POST /Employee?$filter="ID=11"& $method=delete`                                            | Supprime l'entité, collection d'entités ou sélection d'entité courante                                               |
| [**$method=entityset**](#methodentityset)       | `GET /People/?$filter="ID>320"& $method=entityset& $timeout=600`                     | Crée un ensemble d'entités dans le cache de 4D Server basé sur la collection d'entités définies dans la requête REST |
| [**$method=release**](#methodrelease)           | `GET /Employee/$entityset/4CANUMBER?$method=release`                                            | Affiche un ensemble d'entités existant stocké dans le cache de 4D Server                                             |
| [**$method=subentityset**](#methodsubentityset) | `GET /Company(1)/staff?$expand=staff& $method=subentityset&   $subOrderby=lastName ASC` | Crée un ensemble d'entités basé sur la collection d'entités relatives définies dans la requête REST                  |
| [**$method=update**](#methodupdate)             | `POST /Person/?$method=update`                                                                  | Met à jour et/ou crée une ou plusieurs entités                                                                       |
| [**$method=validate**](#methodvalidate)         | `POST /Employee/?$method=validate`                                                              | Valide la demande lors de l'ajout et/ou de la modification d'entités                                                 |


## $method=delete

Supprime l'entité, collection d'entités ou sélection d'entité courante (créée via REST)

### Description

Avec `$method=delete`, vous pouvez supprimer une entité ou une collection d'entités entière. Vous pouvez définir la collection d'entités en utilisant, par exemple, [`$filter`]($filter.md) ou en en spécifiant une directement à l'aide de [`{dataClass}({key})`](%7BdataClass%7D.html#dataclasskey) (par exemple, /Employee(22)).

Vous pouvez également supprimer les entités d'un ensemble d'entités en appelant [`$entityset/{entitySetID}`]($entityset.md#entitysetentitysetid).

## Exemple

Vous pouvez ensuite saisir la requête REST suivante pour supprimer l'entité dont la clé porte le numéro 22 :

`POST  /rest/Employee(22)/?$method=delete`

Vous pouvez également faire une requête en utilisant $ filter :

`POST  /rest/Employee?$filter="ID=11"&$method=delete`

Vous pouvez également supprimer un ensemble d'entités utilisant $entityset/{entitySetID} :

`POST  /rest/Employee/$entityset/73F46BE3A0734EAA9A33CA8B14433570?$method=delete`

Réponse :

    {
        "ok": true
    }
    

## $method=entityset

Crée un ensemble d'entités dans le cache de 4D Server basé sur la collection d'entités définies dans la requête REST

### Description

Lorsque vous créez une collection d'entités dans REST, vous pouvez également créer un ensemble d'entités qui sera enregistré dans le cache de 4D Server. L'ensemble d'entités aura un numéro de référence que vous pouvez passer à `$entityset/{entitySetID}` pour y accéder. Par défaut, il est valable deux heures; vous pouvez toutefois modifier cette durée en passant une valeur (en secondes) à $timeout.

Si vous avez utilisé `$savedfilter` et/ou `$savedorderby` (avec `$filter` et/ou `$orderby`) lors de la création de votre ensemble d'entités, vous pouvez le recréer avec le même ID de référence même s'il a été supprimé du cache de 4D Server.

### Exemple

Pour créer un ensemble d'entités, qui sera enregistré dans le cache de 4D Server pendant deux heures, ajoutez `$method=entityset` à la fin de votre requête REST :

`GET  /rest/People/?$filter="ID>320"&$method=entityset`

Vous pouvez créer un ensemble d'entités qui sera stocké dans le cache de 4D Server pendant seulement dix minutes en passant un nouveau timeout à `$timeout` :

`GET  /rest/People/?$filter="ID>320"&$method=entityset&$timeout=600`

Vous pouvez également enregistrer le filtre et trier, en passant true à `$savedfilter` et `$savedorderby`.

> `$skip` et `$top/$limit` ne sont pas pris en compte lors de l'enregistrement d'un ensemble d'entités.

Après avoir créé un ensemble d'entités, le premier élément, `__ENTITYSET` est ajouté à l'objet retourné et indique l'URI à utiliser pour accéder à l'ensemble d'entités :

`__ENTITYSET: "http://127.0.0.1:8081/rest/Employee/$entityset/9718A30BF61343C796345F3BE5B01CE7"`

## $method=release

Affiche un ensemble d'entités existant stocké dans le cache de 4D Server.

### Description

Vous pouvez afficher un ensemble d'entités, que vous avez créé à l'aide de [`$method=entityset`](#methodentityset), à partir du cache de 4D Server.

### Exemple

Affiche un ensemble d'entités existant :

`GET  /rest/Employee/$entityset/4C51204DD8184B65AC7D79F09A077F24?$method=release`

#### Réponse :

Si la requête a abouti, la réponse suivante est retournée :

    {
        "ok": true
    }
    Si l'entite n'as pas été trouvée, une erreur est retournée :
    
    {
        "__ERROR": [
            {
                 "message": "Error code: 1802\nEntitySet  \"4C51204DD8184B65AC7D79F09A077F24\" cannot be found\ncomponent:  'dbmg'\ntask 22, name: 'HTTP connection handler'\n",
                "componentSignature": "dbmg",
                "errCode": 1802
            }
        ]
    }
    

## $method=subentityset

Crée un ensemble d'entités dans le cache de 4D Server basé sur la collection d'entités relatives définies dans la requête REST

### Description

`$method=subentityset` vous permet de trier les données retournées par l'attribut relationnel défini dans la requête REST.

Pour trier les données, utilisez la propriété `$subOrderby`. Pour chaque attribut, définissez l'ordre sur ASC (ou asc) pour l'ordre croissant et sur DESC (desc) pour l'ordre décroissant. Par défaut, les données sont triées par ordre croissant.

Si vous souhaitez spécifier plusieurs attributs, vous pouvez les délimiter avec une virgule, µ, `$subOrderby="lastName desc, firstName asc"`.

### Exemple

Si vous souhaitez récupérer uniquement les entités relatives pour une entité spécifique, vous pouvez lancer la requête REST suivante, dans laquelle "staff" est l'attribut relationnel dans la dataclass "Company" liée à la dataclass "Employee":

`GET  /rest/Company(1)/staff?$expand=staff&$method=subentityset&$subOrderby=lastName ASC`

#### Réponse :

    {
    
        "__ENTITYSET": "/rest/Employee/$entityset/FF625844008E430B9862E5FD41C741AB",
        "__entityModel": "Employee",
        "__COUNT": 2,
        "__SENT": 2,
        "__FIRST": 0,
        "__ENTITIES": [
            {
                "__KEY": "4",
                "__STAMP": 1,
                "ID": 4,
                "firstName": "Linda",
                "lastName": "Jones",
                "birthday": "1970-10-05T14:23:00Z",
                "employer": {
                    "__deferred": {
                        "uri": "/rest/Company(1)",
                        "__KEY": "1"
                    }
                }
            },
            {
                "__KEY": "1",
                "__STAMP": 3,
                "ID": 1,
                "firstName": "John",
                "lastName": "Smith",
                "birthday": "1985-11-01T15:23:00Z",
                "employer": {
                    "__deferred": {
                        "uri": "/rest/Company(1)",
                        "__KEY": "1"
                    }
                }
            }
        ]
    
    }
    

## $method=update

Met à jour et/ou crée une ou plusieurs entités

### Description

`$method=update` vous permet de mettre à jour et/ou de créer une ou plusieurs entités dans un seul **POST**. Si vous mettez à jour et/ou créez une entité, cela s'effectue dans un objet avec, pour chaque propriété, un attribut et sa valeur, par exemple `{lastName: "Smith"}`. Si vous mettez à jour et/ou créez plusieurs entités, vous devez créer un tableau d'objets.

Pour mettre à jour une entité, vous devez passer les paramètres `__KEY` et `__STAMP` dans l'objet avec tous les attributs modifiés. Si ces deux paramètres sont manquants, une entité sera ajoutée avec les valeurs de l'objet que vous envoyez dans le corps de votre **POST**.

Tous les triggers, attributs calculés et événements sont exécutés immédiatement lors de la sauvegarde de l'entité sur le serveur. La réponse contient toutes les données telles qu'elles existent sur le serveur.

Vous pouvez également placer ces requêtes pour créer ou mettre à jour des entités dans une transaction en appelant `$atomic/$atonce`. Si des erreurs se produisent lors de la validation des données, aucune des entités n'est sauvegardée. Vous pouvez également utiliser $method=validate pour valider les entités avant de les créer ou de les mettre à jour.

Si un problème survient lors de l'ajout ou de la modification d'une entité, une erreur vous sera retournée avec ces informations.

> A noter pour les types d'attributs spécifiques :
> 
> * **Les dates** doivent être exprimées au format JS : YYYY-MM-DDTHH:MM:SSZ (par exemple, "2010-10-05T23:00:00Z"). Si vous avez sélectionné la propriété Date uniquement pour votre attribut Date, le fuseau horaire et l'heure (heure, minutes et secondes) seront supprimés. Dans ce cas, vous pouvez également envoyer la date au format qui vous est retourné dd!mm!yyyy (par exemple, 05!10!2013).
> * Les valeurs des **booléens** sont vrai ou faux.
> * Les fichiers téléchargés à l'aide de `$upload` peuvent s'appliquer à un attribut de type Image ou BLOB en passant l'objet retourné au format suivant {"ID": "D507BC03E613487E9B4C2F6A0512FE50"}

### Exemple

Pour mettre à jour une entité spécifique, utilisez l'URL suivante :

`POST  /rest/Person/?$method=update`

**Données POST :**

    {
        __KEY: "340",
        __STAMP: 2,
        firstName: "Pete",
        lastName: "Miller"
    }
    

Les attributs firstName et lastName de l'entité indiquée ci-dessus seront modifiés en laissant inchangés tous les autres attributs (sauf ceux calculés sur la base de ces attributs).

Si vous souhaitez créer une entité, vous pouvez envoyer, via POST, les attributs à l'aide de cette URL :

`POST  /rest/Person/?$method=update`

**Données POST :**

    { 
        firstName: "John",
        lastName: "Smith"
    }
    

Vous pouvez également créer et mettre à jour plusieurs entités en même temps en utilisant la même URL ci-dessus en passant plusieurs objets d'un tableau au POST :

`POST  /rest/Person/?$method=update`

**Données POST :**

    [{ 
        "__KEY": "309",
        "__STAMP": 5,
        "ID": "309",
        "firstName": "Penelope",
        "lastName": "Miller"
    }, { 
        "firstName": "Ann",
        "lastName": "Jones"
    }]
    

**Réponse :**

Lorsque vous ajoutez ou modifiez une entité, elle vous est retournée avec les attributs qui ont été modifiés. Par exemple, si vous créez le nouvel employé ci-dessus, les informations suivantes seront renvoyées :

    {
        "__KEY": "622", 
        "__STAMP": 1, 
        "uri": "http://127.0.0.1:8081/rest/Employee(622)", 
        "ID": 622, 
        "firstName": "John", 
        "firstName": "Smith",
        "fullName": "John Smith"
    }
    

> La seule raison pour laquelle l'attribut fullName est renvoyé est qu'il s'agit d'un attribut calculé à partir de firstName et lastName.

Si, par exemple, le tampon n'est pas correct, l'erreur suivante est retournée :

    {
        "__ENTITIES": [
            {
                "__KEY": "309", 
                "__STAMP": 1, 
                "ID": 309, 
                "firstName": "Betty", 
                "lastName": "Smith", 
                "fullName": "Betty Smith", 
                "__ERROR": [
                    {
                        "message": "Given stamp does not match current one for record# 308 of table Employee", 
                        "componentSignature": "dbmg", 
                        "errCode": 1263
                    }, 
                    {
                        "message": "Cannot save record 308 in table Employee of database Widgets", 
                        "componentSignature": "dbmg", 
                        "errCode": 1046
                    }, 
                    {
                        "message": "The entity# 308 of the datastore class \"Employee\" cannot be saved", 
                        "componentSignature": "dbmg", 
                        "errCode": 1517
                    }
                ]
            }, 
            {
                "__KEY": "612", 
                "__STAMP": 4, 
                "uri": "http://127.0.0.1:8081/rest/Employee(612)", 
                "ID": 612, 
                "firstName": "Ann", 
                "lastName": "Jones", 
                "fullName": "Ann Jones"
            }
        ]
    }
    

Si, par exemple, l'utilisateur ne dispose pas des autorisations nécessaires pour mettre à jour une entité, l'erreur suivante est retournée :

    {
        "__KEY": "2", 
        "__STAMP": 4, 
        "ID": 2, 
        "firstName": "Paula", 
        "lastName": "Miller", 
        "fullName": "Paula Miller", 
        "telephone": "408-555-5555", 
        "salary": 56000, 
        "employerName": "Adobe", 
        "employer": {
            "__deferred": {
                "uri": "http://127.0.0.1:8081/rest/Company(1)", 
                "__KEY": "1"
            }
        }, 
        "__ERROR": [
            {
                "message": "No permission to update for dataClass Employee", 
                "componentSignature": "dbmg", 
                "errCode": 1558
            }, 
            {
                "message": "The entity# 1 of the datastore class \"Employee\" cannot be saved", 
                "componentSignature": "dbmg", 
                "errCode": 1517
            }
        ]
    }
    

## $method=validate

Valide la demande lors de l'ajout et/ou de la modification d'entités

### Description

Avant d'enregistrer une entité nouvelle ou modifiée à l'aide de `$method=update`, vous pouvez d'abord essayer de valider les actions avec `$method=validate`.

### Exemple

Dans cet exemple, nous envoyons via **POST** la requête suivante à $method=validate :

`POST  /rest/Employee/?$method=validate`

**POST data**:

    [{
        "__KEY": "1",
        "__STAMP": 8,
        "firstName": "Pete",
        "lastName": "Jones",
        "salary": 75000
    }, {
        "firstName": "Betty",
        "lastName": "Miller",
    }]
    

**Réponse** :

Si la requête a abouti, la réponse suivante est retournée :

    {
        "ok": true
    }
    

Sinon, vous recevez une erreur. Dans notre cas, une erreur a été retournée car notre champ "salary" doit être inférieur à 60000 :

    {
        "__ENTITIES": [
            {
                "__ERROR": [
                    {
                        "message": "Value cannot be greater than 60000", 
                        "componentSignature": "dbmg", 
                        "errCode": 1569
                    }, 
                    {
                        "message": "Entity fails validation", 
                        "componentSignature": "dbmg", 
                        "errCode": 1570
                    }, 
                    {
                        "message": "The new entity of the datastore class \"Employee\" cannot be saved", 
                        "componentSignature": "dbmg", 
                        "errCode": 1534
                    }
                ]
            }
        ]
    }
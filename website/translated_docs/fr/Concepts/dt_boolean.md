---
id: boolean
title: Booléen
---

Un champ, une variable ou une expression de type booléen peut être soit VRAI soit FAUX.

## Fonctions booléennes

Les fonctions booléennes de 4D traitent des valeurs telles que `Vrai`, `Faux` et `Non` dans le thème **Booléens** consacré. Pour plus d'informations, veuillez vous reporter à la description de ces commandes.

### Exemple

L'exemple suivant retourne Vrai dans la variable monBooléen si l'utilisateur a cliqué sur le bouton monBouton et Faux s'il n'a pas cliqué dessus. . Lorsqu'un bouton reçoit un clic, la variable du bouton prend la valeur 1.

```4d
 If(myButton=1) //If the button was clicked
    myBoolean:=True //myBoolean is set to True
 Else //If the button was not clicked,
    myBoolean:=False //myBoolean is set to False
 End if
```

L'exemple ci-dessus peut être simplifié et écrit en une seule ligne .

```4d
monBooléen:=(monBouton=1)
```

## Opérateurs logiques

4D supporte deux opérateurs logiques : l'opérateur d'intersection (AND) et l'opérateur de réunion inclusive (OR). Le AND logique retourne TRUE si les deux expressions sont VRAIES. Le OR logique retourne TRUE si au moins une des expressions est VRAIE. Le tableau suivant décrit les opérateurs logiques :

| Opération | Syntaxe           | Retourne | Expression                  | Valeur |
| --------- | ----------------- | -------- | --------------------------- | ------ |
| AND       | Booléen & Booléen | Booléen  | ("A" = "A") & (15 # 3)      | Vrai   |
|           |                   |          | ("A" = "B") & (15 # 3)      | Faux   |
|           |                   |          | ("A" = "B") & (15 = 3)      | Faux   |
| OU        | Booléen & Booléen | Booléen  | ("A" = "A") &#124; (15 # 3) | Vrai   |
|           |                   |          | ("A" = "B") &#124; (15 # 3) | Vrai   |
|           |                   |          | ("A" = "B") &#124; (15 = 3) | Faux   |


Voici la "table de vérité" pour l'opérateur logique "AND" :

| Expr1 | Expr2 | Expr1 & Expr2 |
| ----- | ----- | ------------- |
| Vrai  | Vrai  | Vrai          |
| Vrai  | Faux  | Faux          |
| Faux  | Vrai  | Faux          |
| Faux  | Faux  | Faux          |


Voici la "table de vérité" pour l'opérateur logique "OR" :

| Expr1 | Expr2 | Expr1 &#124; Expr2 |
| ----- | ----- | ------------------ |
| Vrai  | Vrai  | Vrai               |
| Vrai  | Faux  | Vrai               |
| Faux  | Vrai  | Vrai               |
| Faux  | Faux  | Faux               |


**Astuce :** Si vous devez calculer une réunion exclusive (le "OU" exclusif) entre Expr1 et Expr2, écrivez :

```4d
 (Expr1|Expr2) & Not(Expr1 & Expr2)  
```
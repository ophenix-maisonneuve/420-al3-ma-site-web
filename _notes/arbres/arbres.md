---
layout: default
title: "Arbres"
parent: "Structures de données"
nav_order: 2
published: true
---

# Arbres

Une **structure d'arbre** est une structure de données hiérarchique dans laquelle chaque élément (appelé **noeud**) est relié à un ou plusieurs noeuds enfants, sauf le noeud racine qui est unique. Cette structure est utilisée pour représenter des relations hiérarchiques comme les systèmes de fichiers, les arbres syntaxiques, ou les arbres de décision.

## Caractéristiques principales
- **Hiérarchique** : chaque noeud peut avoir plusieurs enfants mais un seul parent.
- **Racine** : point de départ de l'arbre.
- **Feuilles** : noeuds sans enfants.
- **Sous-arbres** : chaque noeud peut être la racine d'un sous-arbre.

## Types d'opérations principales
- **Insertion** : ajouter un noeud à une position spécifique.
- **Suppression** : retirer un noeud et réorganiser l'arbre.
- **Recherche** : localiser un noeud selon une valeur.
- **Parcours** : visiter les noeuds selon un ordre défini (préfixe, infixe, suffixe).

---

## Types d'arbres

| Type d'arbre            | Nombre d'enfants max | Organisation des données | Recherche rapide | Insertion/Suppression efficace |
|-------------------------|----------------------|---------------------------|------------------|-------------------------------|
| Arbre binaire de recherche | 2                    | Oui                       | Oui              | Oui                           |


{: .highlight}
> D'autres types d'arbres seront ajoutés ici dans les prochaines semaines...

---

## Opérations sur les arbres

| Opération     | Complexité moyenne | Complexité pire cas | Remarques |
|---------------|--------------------|----------------------|-----------|
| Insertion     | O(log n)           | O(n)                 | Si l'arbre est équilibré |
| Suppression   | O(log n)           | O(n)                 | Nécessite parfois rééquilibrage |
| Recherche     | O(log n)           | O(n)                 | Dépend de la forme de l'arbre |
| Parcours      | O(n)               | O(n)                 | Tous les noeuds sont visités |

---

## Exemples de cas d'utilisation
- Systèmes de fichiers
- Arbres de décision
- Arbres syntaxiques pour compilateurs
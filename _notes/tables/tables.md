---
layout: default
title: "Tables associatives"
parent: "Structures de données"
nav_order: 3
published: true
---

# Tables associatives

Une **table associative** (aussi appelée **map** ou **dictionnaire**) est une structure de données qui associe des **clés uniques** à des **valeurs**. Elle permet des accès rapides aux données via une clé. Les deux implémentations les plus fréquentes sont :

- **Tables de hachage** : utilisent une fonction de hachage pour calculer un index à partir de la clé. Elles offrent des performances moyennes proches de O(1) pour l'insertion, la recherche et la suppression, mais nécessitent une **gestion des collisions** (ex. chaînage séparé, sondage linéaire, double hachage).
- **Tables basées sur des arbres (*tree-based maps*)** : utilisent des arbres équilibrés (ex. rouge-noir) pour maintenir les clés triées, avec des opérations en O(log n). Elles ne nécessitent pas de gestion des collisions.

![Illustration d'une table associative](../assets/images/associative-array.png)

## Caractéristiques principales
- **Clés uniques** : chaque clé est associée à une seule valeur.
- **Accès rapide** : les tables associatives sont particulièrement rapides pour la recherche d'une valeur par sa clé.

## Types d'opérations principales
- **Insertion (put)** : ajoute ou remplace une paire clé-valeur.
- **Recherche (get)** : récupère la valeur associée à une clé.
- **Suppression (remove)** : supprime une paire clé-valeur.
- **Redimensionnement (resize)** : augmente la capacité pour réduire les collisions.


## Types de tables associatives

L'association entre une clé et une valeur peut être réalisée de plusieurs façons :

- **Table de hachage (*Hash Table*)** :
  - Utilise une fonction de hachage pour calculer un index à partir de la clé.
  - Avantages : accès en temps moyen O(1).
  - Inconvénients : collisions possibles, nécessite une bonne fonction de hachage.

- **Tables basées sur des arbres (*tree-based maps*)** :
  - Utilise des structures d'arbres (par exemple, arbres binaires de recherche, arbres équilibrés comme AVL ou Rouge-Noir).
  - Avantages : maintien des clés triées, complexité O(log n).
  - Inconvénients : plus complexe à implémenter.

- **Structures hybrides** :
  - Combinaison de hachage et d'arbres pour optimiser les performances (ex. : HashMap avec arbres pour gérer les collisions).

Chaque méthode présente des compromis entre vitesse, mémoire et complexité d'implémentation.

---

## Tableau comparatif

| Structure            | Accès direct | Insertion rapide | Gestion des doublons |
|----------------------|--------------|------------------|-----------------------|
| Tableau             | Oui          | Non              | Non                   |
| Liste chaînée       | Non          | Oui              | Oui                   |
| Table associative   | Oui (par clé)| Oui              | Non                   |

---

## Opérations et complexité

| Opération   | Table de hachage (moyenne / pire cas) | Table basée sur un arbre (Tree-based Map) |
|-------------|----------------------------------------|--------------------------------------------|
| Insertion (*put*)         | O(1) / O(n) (si collisions ou redimensionnement) | O(log n) |
| Recherche (*get*, *containsKey*) | O(1) / O(n) (si collisions nombreuses)         | O(log n) |
| Suppression (*remove*)     | O(1) / O(n) (si collisions nombreuses)             | O(log n) |

{: .highlight}
> Les tables de hachage sont plus rapides en moyenne, mais ne garantissent pas d’ordre sur les clés.  
> Les tables basées sur des arbres maintiennent les clés triées, avec des performances logarithmiques garanties.

---

## Cas d'utilisation
- Indexation en base de données
- Caches mémoire
- Dictionnaires
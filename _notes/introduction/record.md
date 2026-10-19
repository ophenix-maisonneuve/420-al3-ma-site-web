---
layout: default
title: "Type record"
parent: "Introduction à Java et ses outils"
nav_order: 3
published: true
---

# Type `record` en Java

Depuis Java 16, le mot-clé `record` permet de définir des **classes de données immuables** de manière concise. Il est particulièrement utile pour les objets qui servent principalement à **stocker des données**.

---

## Déclaration d'un `record`

```java
public record Vehicule(String marque, int annee) {}
```

Cette ligne de code génère automatiquement :

- Des **champs privés et finaux** (`private final String marque`, `private final int annee`)
- Un **constructeur** avec tous les paramètres
- Des **accesseurs** sans préfixe `get` : `marque()` et `annee()`
- Les méthodes :
  - `equals(Object o)`
  - `hashCode()`
  - `toString()`

---

## Exemple d'utilisation

```java
Vehicule v1 = new Vehicule("Toyota", 2022);
System.out.println(v1.marque());      // Affiche: Toyota
System.out.println(v1.annee());       // Affiche: 2022
System.out.println(v1);               // Affiche: Vehicule[marque=Toyota, annee=2022]
```

---

## Pourquoi utiliser les `records` ?

- Moins de code à écrire
- Encapsulation automatique
- Immuabilité par défaut
- Idéal pour les DTOs, les clés de map, les résultats de requêtes, etc.

---

## Limitations

- Les `record` ne peuvent pas étendre une autre classe (ils étendent implicitement `java.lang.Record`)
- Tous les champs sont **final** (non modifiables)
- Mal adaptés aux entités complexes avec logique métier

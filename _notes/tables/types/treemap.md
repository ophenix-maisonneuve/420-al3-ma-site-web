---
title: "java.util.TreeMap"
layout: default
parent: "java.util.Map"
nav_order: 1
published: true
---

# java.util.TreeMap

![Schéma TreeMap](../assets/images/treemap.png)

## Description
`TreeMap` est une implémentation de `Map` **triée** basée sur un **arbre rouge‑noir** (auto‑équilibré). Les opérations courantes (`get`, `put`, `remove`, `containsKey`) sont en **O(log n)**, l’ordre des clés étant déterminé par l’**ordre naturel** ou un **`Comparator`** fourni au constructeur.

## Fonctionnement interne
`TreeMap` maintient les entrées dans un **arbre rouge‑noir** : les rééquilibrages par recoloriage et **rotations** garantissent une hauteur logarithmique et donc des performances prévisibles. Les vues de navigation (`firstKey`, `lastKey`, `floorKey`, `ceilingKey`, `subMap`, `headMap`, `tailMap`) exploitent la structure triée.

---

## Forces et faiblesses

### Forces
- **Ordre garanti** (parcours trié, accès aux bornes, sous‑plages).
- **Complexité logarithmique** pour `get`/`put`/`remove`.
- **Comparateur personnalisé** pour des tris multi‑critères.

### Faiblesses
- **Plus coûteux** que `HashMap` lorsqu’un ordre n’est pas nécessaire.
- **Contraintes de comparabilité** : les clés doivent être **mutuellement comparables**, soit par leur ordre naturel ou avec un `Comparator` fourni.
- **Non thread‑safe** : utiliser un wrapper (`Collections.synchronizedSortedMap`) au besoin.

---

## Quand l’utiliser ?
Quand l’**ordre** des clés est **fondamental** (classements, intervalles, navigation). Si les accès aléatoires sans ordre priment, préférer `HashMap`.

{: .astuce}
> Besoin d’un **ensemble trié** de clés et d’opérations **logarithmiques** : `TreeMap`.
> Besoin de **performances brutes** sans ordre : `HashMap`.

---

## Complexité

| **Opération**                                      | **Complexité** |
|----------------------------------------------------|-----------------|
| `get`                                             | O(log n)       |
| `put`                                             | O(log n)       |
| `remove`                                          | O(log n)       |
| `containsKey`                                     | O(log n)       |
| `containsValue`                                   | O(n)           |
| `firstKey`, `lastKey`, `floorKey`, `ceilingKey`   | O(log n)       |
| `iterator()` (parcours trié)                      | O(n)           |


---

## Exemples de code

<details markdown="1">
<summary markdown="span">Création et ordre naturel</summary>

```java
import java.util.*;

public class DemoTreeMap {
    public static void main(String[] args) {
        Map<String, Integer> classement = new TreeMap<>();
        classement.put("alice", 120);
        classement.put("bob", 180);
        classement.put("charlie", 150);

        for (Map.Entry<String, Integer> e : classement.entrySet()) {
            System.out.println(e.getKey() + " => " + e.getValue());
        }
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">Ordre personnalisé via `Comparator`</summary>

```java
import java.util.*;

class Joueur {
    private String pseudo;
    private int score;

    public Joueur(String pseudo, int score) {
        this.pseudo = pseudo;
        this.score = score;
    }

    public String getPseudo() {
        return pseudo;
    }

    public int getScore() {
        return score;
    }

    @Override
    public String toString() {
        return pseudo + " (" + score + ")";
    }
}

public class DemoTreeMapComparator {
    public static void main(String[] args) {
        Comparator<Joueur> parScoreDescPuisPseudo = new Comparator<Joueur>() {
            @Override
            public int compare(Joueur a, Joueur b) {
                if (a.getScore() != b.getScore()) {
                    return Integer.compare(b.getScore(), a.getScore());
                }
                return a.getPseudo().compareTo(b.getPseudo());
            }
        };

        Map<Joueur, Integer> classement = new TreeMap<>(parScoreDescPuisPseudo);
        classement.put(new Joueur("alice", 120), 120);
        classement.put(new Joueur("bob", 180), 180);
        classement.put(new Joueur("charlie", 180), 180);

        for (Map.Entry<Joueur, Integer> e : classement.entrySet()) {
            System.out.println(e.getKey());
        }
    }
}
```
</details>

---

## Voir aussi
- [Documentation `TreeMap` officielle](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeMap.html)

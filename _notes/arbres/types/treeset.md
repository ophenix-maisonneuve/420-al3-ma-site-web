---
title: "java.util.TreeSet"
layout: default
parent: "Arbres"
nav_order: 3
published: true
---

# java.util.TreeSet

## Description
`TreeSet` est une implémentation de l’interface `Set` **triée** et **sans doublons**, basée sur une structure d’**arbre de recherche équilibré** (en Java, un [arbre rouge‑noir](../notes/arbre-rouge-noir)). Les éléments sont maintenus dans l’**ordre naturel** ou selon un **`Comparator`** fourni au constructeur. Les opérations fondamentales (`add`, `remove`, `contains`) sont en **O(log n)** et le parcours se fait **en ordre trié**.

## Fonctionnement interne
`TreeSet` n'est en réalité rien de plus qu'une `TreeMap` dans laquelle les **clés** correspondent aux **éléments** (les valeurs ne sont pas considérées et contiennent généralement une valeur par défaut constante). Ainsi, l’ordre et la navigation (bornes, sous‑ensembles) proviennent des opérations de `NavigableMap` : `first`, `last`, `ceiling`, `floor`, `headSet`, `tailSet`, `subSet`.

---

## Forces et faiblesses

### Forces
- **Ordre garanti** : itération triée, accès aux **bornes** (`first`, `last`).
- **Navigation puissante** : **vues** par intervalles (`headSet`, `tailSet`, `subSet`) et méthodes de plafond/plancher (`ceiling`, `floor`).
- **Comparateurs** : permet des **tris personnalisés** (multi‑critères, ordre inversé, etc).

### Faiblesses
- **Plus lent** qu’un `HashSet` pour des accès bruts (O(log n) vs O(1) amorti).
- **Contraintes de comparabilité** : les éléments doivent être **mutuellement comparables**, soit par leur ordre naturel ou par un `Comparator` fourni.
- **Non thread‑safe** : utiliser un wrapper (`Collections.synchronizedSortedSet`)

---

## Quand l’utiliser ?
Lorsque l’**ordre** des éléments est **essentiel** (classements, index triés, fenêtres d’intervalle) et que l’on veut des **coûts logarithmiques** garantis pour les mises à jour et les recherches. Si l’ordre n’est pas requis et que l’on priorise les accès les plus rapides, préférer `HashSet`. 

{: .astuce}
> Besoin d’un **ensemble trié** avec navigation par **plages** ? Utiliser un `TreeSet`.
> Besoin de **vitesse brute** sans ordre ? Utiliser un `HashSet`.

---

## Complexité des opérations


| **Opération**                     | **Complexité** |
|-----------------------------------|-----------------|
| `add(E e)`                        | O(log n)       |
| `remove(Object o)`                | O(log n)       |
| `contains(Object o)`              | O(log n)       |
| `first()` / `last()`              | O(log n)       |
| `iterator()` (parcours)           | O(n)           |
| `subSet(fromElement, toElement)`  | O(log n) pour créer le sous-ensemble, O(n) pour itérer |
| `headSet(toElement)`              | O(log n) pour créer le sous-ensemble, O(n) pour itérer |
| `tailSet(fromElement)`            | O(log n) pour créer le sous-ensemble, O(n) pour itérer |

*Si on veut être plus précis, on pourrait dire que l'itération sur les sous-ensemble est O(k) où k représente le nombre d'éléments dans le sous-ensemble. On reste cependant dans l'ordre de grandeur O(n).*


---

## Exemples de code

<details markdown="1">
<summary markdown="span">Création et ordre naturel</summary>

```java
import java.util.*;

public class DemoTreeSet {
    public static void main(String[] args) {
        Set<String> pseudos = new TreeSet<>();
        pseudos.add("alice");
        pseudos.add("bob");
        pseudos.add("charlie");
        pseudos.add("bob"); // Ignoré : doublon

        // Parcours trié (ordre naturel)
        for (String p : pseudos) {
            System.out.println(p);
        }

        String min = ((TreeSet<String>) pseudos).first();
        String max = ((TreeSet<String>) pseudos).last();
        System.out.println("Min: " + min + ", Max: " + max);
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">Ordre personnalisé avec `Comparator`</summary>

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

public class DemoTreeSetComparator {
    public static void main(String[] args) {
        Comparator<Joueur> parScoreDescPuisPseudo = Comparator.comparing(Joueur::getScore)
                .reversed()
                .thenComparing(Joueur::getPseudo);

        Set<Joueur> classement = new TreeSet<>(parScoreDescPuisPseudo);
        classement.add(new Joueur("alice", 120));
        classement.add(new Joueur("bob", 180));
        classement.add(new Joueur("charlie", 180)); // tie-break sur pseudo

        for (Joueur j : classement) {
            System.out.println(j);
        }
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">Sous‑ensembles (`subSet`, `headSet`, `tailSet`)</summary>

```java
import java.util.*;

public class DemoSubsets {
    public static void main(String[] args) {
        TreeSet<Integer> valeurs = new TreeSet<>();
        valeurs.add(5);
        valeurs.add(1);
        valeurs.add(9);
        valeurs.add(3);
        valeurs.add(7);

        // intervalle [3, 8)
        SortedSet<Integer> intervalle = valeurs.subSet(3, 8);
        System.out.println("Intervalle: " + intervalle);

        // valeurs strictement inférieures à 5
        SortedSet<Integer> heads = valeurs.headSet(5);
        System.out.println("HeadSet: " + heads);

        // valeurs >= 7
        SortedSet<Integer> tails = valeurs.tailSet(7);
        System.out.println("TailSet: " + tails);
    }
}
```
</details>

---

## Voir aussi
- [java.util.TreeSet (Oracle JDK 25)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeSet.html)
- [java.util.TreeMap (Oracle JDK 25)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeMap.html)

{: .highlight}
> `TreeSet` fait partie du **Collections Framework** et exploite les **génériques** (`Set<E>`). Pour rappel, consultez la page suivante: [Génériques en Java](../notes/generiques-java)

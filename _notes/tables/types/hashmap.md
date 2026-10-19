---
title: "java.util.HashMap"
layout: default
parent: "java.util.Map"
nav_order: 2
published: true
---

# java.util.HashMap

![Schéma HashMap](../assets/images/hash-table-chainage-simple.png)

`HashMap` est une implémentation de table associative **basée sur une table de hachage**. Elle offre des performances **en temps constant O(1) amorti** pour `get` et `put` lorsque la fonction de hachage répartit bien les clés. L’ordre d’itération **n’est pas garanti** et peut changer au fil des opérations. Elle **autorise** une seule **clé `null`** ainsi que les **valeurs `null`**. 

`HashSet` est une implémentation de l’interface `Set` **utilisant une `HashMap`** interne (les éléments du *set* servent de clés). Elle **n’autorise aucun doublon**, **ne garantit pas l’ordre d’itération**, et **autorise l’élément `null`**. Les opérations fondamentales (`add`, `remove`, `contains`) sont **en temps constant amorti** si le hachage est de bonne qualité.

## Fonctionnement interne

**HashMap**  
La table est composée de **cases** (*buckets*). Deux paramètres influencent les performances :
- **Capacité initiale** (nombre de cases), par défaut `16`
- **Facteur de charge** (*load factor*), par défaut `0.75` : lorsque `taille > capacité × facteur`, la table **redimensionne** et **rehache** les entrées (en général, capacité doublée).

**HashSet**  
`HashSet` utilise **une `HashMap`** comme structure sous-jacente (chaque élément est stocké comme clé et les valeurs restent `null`). Il **hérite** donc des caractéristiques de performance, de capacité et de facteur de charge de `HashMap` (incluant l’impact de la **capacité** et du **load factor** sur le coût d’itération).

Pour gérer les collisions (deux clés différentes qui produisent le même index dans le tableau), `HashMap` et `HashSet` utilisent généralement une liste doublement chaînée. Cependant, depuis Java 8, les cases fortement en collision peuvent basculer vers des **arbres équilibrés** afin de conserver de bonnes performances (selon la comparabilité des clés). Par défaut, Java passe la case en **arbre rouge-noir** lorsque le nombre d'éléments **dépasse 8** et revient à une liste doublement chaînée lorsque le nombre d'éléments **redevient inférieur à 6**.

---

## Forces et faiblesses

### HashMap
**Forces**
- **Accès rapide** (`get`, `put`) **O(1)** amorti.  
- **Souplesse** : **clé** et **valeur** peuvent être `null`.  
- **API riche** (`compute*`, `merge`, `putIfAbsent`, etc.).

**Faiblesses**
- **Ordre non garanti** (contrairement à `TreeMap`).  
- **Redimensionnements** et **rehachage** peuvent être coûteux.  
- **Non thread‑safe** (utiliser `Collections.synchronizedMap` ou `ConcurrentHashMap`).

### HashSet
**Forces**
- **Unicité des éléments** (aucun doublon).  
- **Opérations de base** (`add`, `remove`, `contains`) en **O(1)** amorti.  
- **Autorise `null`** comme élément.

**Faiblesses**
- **Ordre non garanti** (contrairement à `TreeSet`).
- **Redimensionnements** et **rehachage** peuvent être coûteux.
- **Non thread‑safe** (utiliser `Collections.synchronizedSet`).

---

## Quand les utiliser ?

- **`HashMap`** : lorsque la **vitesse** pour `get`/`put` prime et que l’**ordre** n’est pas requis.  
- **`HashSet`** : lorsque l’objectif est d’**assurer l’unicité** d’éléments et de tester rapidement l’appartenance.

{: .astuce}
> Besoin d’un **ensemble non trié** de paires clé/valeur et **accès très rapide** : `HashMap`.  
> Besoin d’un **ensemble d’éléments uniques** sans ordre : `HashSet`.  
> Si l’ordre d’insertion ou un ordre trié est nécessaire, voir `TreeMap`/`TreeSet`.

---

## Complexité

### HashMap

| **Opération**           | **Complexité (moyenne)** |
|-------------------------|---------------------------|
| `get`                   | O(1) amorti               |
| `put`                   | O(1) amorti               |
| `remove`                | O(1) amorti               |
| `containsKey`           | O(1) amorti               |
| `containsValue`         | O(n)                      |
| `iterator()` (parcours) | O(n)           |

### HashSet

| **Opération**          | **Complexité (moyenne)** |
|------------------------|---------------------------|
| `add(E e)`             | O(1) amorti               |
| `remove(Object o)`     | O(1) amorti               |
| `contains(Object o)`   | O(1) amorti               |
| `iterator()` (parcours)| O(n)                      |

---

## Exemples de code

### HashMap — comptage de fréquences avec `merge`
```java
import java.util.*;

public class DemoHashMap {
    public static void main(String[] args) {
        Map<String, Integer> freq = new HashMap<>();
        String[] mots = new String[] {"un", "deux", "un", "trois", "deux"};

        for (String m : mots) {
            // Accumule la fréquence en O(1) amorti par insertion/MAJ
            freq.merge(m, 1, new java.util.function.BiFunction<Integer, Integer, Integer>() {
                @Override
                public Integer apply(Integer a, Integer b) {
                    return a + b;
                }
            });
        }

        System.out.println(freq);
    }
}
```

### HashSet — filtrer les doublons en entrée
```java
import java.util.*;

public class DemoHashSet {
    public static void main(String[] args) {
        Set<Integer> uniques = new HashSet<>();
        int[] data = new int[] {1, 2, 1, 3, 2};

        for (int x : data) {
            uniques.add(x); // O(1) amorti ; les doublons sont ignorés
        }

        System.out.println("Taille: " + uniques.size()); // 3
        System.out.println("Éléments: " + uniques);       // ordre non garanti
    }
}
```

## Voir aussi
- [Documentation `HashMap` officielle](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashMap.html)
- [Documentation `HashSet` officielle](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashSet.html)

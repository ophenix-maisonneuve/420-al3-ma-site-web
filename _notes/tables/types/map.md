---
layout: default
parent: "Tables associatives"
title: "java.util.Map"
nav_order: 2
has_toc: true
published: true
---

# Interface `java.util.Map`

## Description
Une **table associative** (*map*) associe des **clés** à des **valeurs** et **n'autorise pas les clés dupliquées** : une clé ne peut référencer qu'une seule valeur. L'interface fournit des **vues** de la collection — `keySet()` (ensemble des clés), `values()` (collection des valeurs) et `entrySet()` (ensemble des paires clé/valeur). L'**ordre d'itération** dépend de l'implémentation : certaines préservent un ordre (p. ex. `TreeMap`), d’autres non (p. ex. `HashMap`).

## Quand l'utiliser ?
Utilisez une **table associative** lorsque vous devez **retrouver rapidement une valeur à partir d'une clé** et gérer des **paires clés/valeurs** avec des opérations d'ajout, de remplacement, de suppression et de parcours. Une `Map` convient lorsque l'unicité des clés est essentielle et que la structure doit exposer des vues (`keySet`, `values`, `entrySet`).

## Méthodes principales

| **Méthode**                                                      | **Description**                                                                 | **Remarques**                                                   |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------|-----------------------------------------------------------------|
| `get(K key)`                                                     | Retourne la valeur associée à la clé (ou `null` si absente).                   | La signification de `null` dépend de l'implémentation.          |
| `put(K key, V value)`                                            | Associe la clé à la valeur (remplace la valeur précédente si la clé existait). | Opération **destructive** (modifie la structure).              |
| `remove(Object key)`                                             | Supprime le mapping pour la clé si présent.                                    | Retourne l’ancienne valeur ou `null`.                          |
| `containsKey(Object key)` / `containsValue(Object value)`        | Teste la présence d’une clé / d’une valeur.                                    | Selon l’implémentation, la recherche par valeur peut être **linéaire**. |
| `size()` / `isEmpty()`                                           | Nombre d’entrées / test si la map est vide.                                    | En contexte concurrent, ces valeurs peuvent être **approximatives**. |
| `keySet()` / `values()` / `entrySet()`                           | Vues sur les clés, les valeurs, ou les *entries*.                              | Les vues sont **liées** à la map (modifications répercutées).  |
| `getOrDefault(K key, V defaultValue)`                            | Retourne la valeur ou une valeur par défaut si absente.                        | Pratique pour éviter un test explicite.                        |
| `putIfAbsent(K key, V value)`                                    | Ajoute uniquement si la clé n’est pas déjà associée.                           | Utile pour initialiser un mapping.                             |
| `replace(K key, V newValue)` / `replace(K key, V oldValue, V newValue)` | Remplace la valeur associée à la clé (ou conditionnellement).                  | Ne modifie pas la cardinalité.                                 |
| `compute`, `computeIfAbsent`, `computeIfPresent`                | Calcule/actualise une valeur à partir d’une fonction.                          | Rend les mises à jour **atomiques** au niveau logique.         |
| `merge(K key, V value, BiFunction<V,V,V> fn)`                   | Combine une valeur existante avec une nouvelle valeur via une fonction.        | Pratique pour accumuler des totaux ou des listes.              |

{: warning}
> Lorsqu'une *map* contient des collections comme valeurs (par exemple, une Map<Integer, List>), l'itération se fait généralement à l'aide de deux boucles imbriquées : la première parcourt les clés, la seconde parcourt les éléments de la liste associée à chaque clé. Bien que deux boucles soient utilisées, la complexité reste O(n) et non O(n<sup>2</sup>), car chaque élément est visité une seule fois au total.


## Exemple
```java
import java.util.*;

public class DemoMap {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();
        scores.put("alice", 120);
        scores.put("bob", 180);
        scores.putIfAbsent("charlie", 150);

        int v = scores.getOrDefault("david", 0);
        System.out.println("Score de david: " + v);

        // Accumulation avec merge
        scores.merge("alice", 30, new java.util.function.BiFunction<Integer,Integer,Integer>() {
            @Override
            public Integer apply(Integer a, Integer b) {
                return a + b;
            }
        });
    }
}
```

## À voir aussi
- [Interface `Map` (documentation officielle)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Map.html)
- [Aperçu Collections Framework](https://docs.oracle.com/en/java/javase/25/core/java-collections-framework.html)


{: .highlight}
> Les **tables associatives** Java exploitent largement les **génériques** (`Map<K,V>`). Pour rappel, revoir la page suivante: [Génériques en Java](../notes/generiques-java)


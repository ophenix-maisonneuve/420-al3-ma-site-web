---
title: "java.util.concurrent.ConcurrentHashMap"
layout: default
parent: "java.util.Map"
nav_order: 3
published: true
---

# java.util.concurrent.ConcurrentHashMap

## Description
`ConcurrentHashMap` est une table de hachage **thread‑safe** offrant une **pleine concurrence en lecture** et une **forte concurrence en écriture**. Les **lectures** (dont `get`) sont en général **non bloquantes** et peuvent **chevaucher** des mises à jour (`put`, `remove`). Les itérateurs sont **faiblement cohérents** (ne lèvent pas `ConcurrentModificationException`).

## Fonctionnement interne
La structure **redimensionne** dynamiquement lorsque les collisions deviennent trop nombreuses (seuil lié au **facteur de charge ~0.75**). Les opérations de lecture n’emploient pas de verrou global; les mises à jour utilisent des mécanismes de concurrence fins pour préserver la **sécurité des threads** tout en maximisant le débit. Il s'agit d'un mélange d'opérations atomiques (*CAS – Compare And Swap*) et de verrous localisés (par case, jamais global) pour certaines opérations. Les **clés `null`** et les **valeurs `null`** **ne sont pas autorisées** (pour éviter l’ambiguïté entre « clé absente » et « valeur `null` » en environnement concurrent).

---

## Forces et faiblesses

### Forces
- **Lectures non bloquantes**; **itérateurs faiblement cohérents**.
- **Très bonnes performances** sous forte concurrence.
- Méthodes atomiques utiles: `putIfAbsent`, `replace`, `compute`, `merge`.

### Faiblesses
- **Ordre d’itération non garanti**.
- **Coût** des redimensionnements; éviter les tailles initiales trop faibles.
- Certaines méthodes d’agrégat (`size`, `containsValue`) reflètent des **états transitoires**.

---

## Quand l’utiliser ?
Lorsque **plusieurs threads** doivent lire et modifier simultanément une table associative **sans verrou global** (caches, index partagés, compteurs).

{: .warning}
> `ConcurrentHashMap` **n’autorise pas** les **clés/valeurs `null`**. Utilisez une **valeur sentinelle** ou encapsulez avec `Optional` si nécessaire.

---

## Complexité

**Opération**  | **Complexité (attendue)**
-------------- | -------------------------
`get`          | O(1) amorti
`put`          | O(1) amorti
`remove`       | O(1) amorti
`containsKey`  | O(1) amorti
`containsValue`| O(n)
`iterator()` (parcours faiblement cohérent) | O(n)

---

## Comparaison avec `Collections.synchronizedMap`

### Description rapide
**`Collections.synchronizedMap`** est un *wrapper* qui rend une `Map` (souvent une `HashMap`) **thread‑safe** via un **verrou unique** (*mutex*) autour de **toutes** les opérations. Les itérateurs **doivent** être utilisés à l’intérieur d’un bloc `synchronized` sur la map.

**Forces**
- **Simplicité** : transformer rapidement une `Map` non *thread‑safe* en *thread‑safe*.
- **Sémantique forte** par opération : chaque appel est **entièrement synchronisé** (état observé **cohérent** au moment de l’appel).
- **Autorise `null`** si la map sous‑jacente l’autorise (ex. `HashMap`).

**Faiblesses**
- **Verrou global** : **forte contention** dès que plusieurs threads accèdent/écrivent : **supporte mal la montée en charge**.
- Les **itérateurs ne sont pas thread‑safe** : il faut **synchroniser manuellement** pendant l’itération (`synchronized(map) { for (...) ... }`).
- **Performances inférieures**** sous charge par rapport aux structures conçues pour la concurrence.

### Comment choisir ?
- **Utiliser `ConcurrentHashMap` si :**
  - Plusieurs threads **lisent et écrivent** fréquemment et simultanément.
  - Besoin d’un **débit élevé** et d’une **latence faible** sous **forte concurrence**.
  - Exploitation des **opérations atomiques** (`putIfAbsent`, `compute`, `merge`) ou des **opérations bulk** concurrentes (`forEach`, `reduce`, `search`).
  - Les **clés/valeurs `null`** ne sont pas nécessaires.

- **Utiliser `Collections.synchronizedMap` si :**
  - Concurrence **faible** ou **rare** (accès ponctuels).
  - Besoin de **sécuriser rapidement** une `Map` existante sans changer d’implémentation.
  - Besoin d’une **cohérence forte par opération** et contrôle de l’itération via des blocs `synchronized`.
  - Nécessité d’**autoriser `null`** (si supporté par la map sous‑jacente).

---

## Exemples de code

## `ConcurrentHashMap` - usage général
```java
import java.util.concurrent.*;
public class DemoConcurrentHashMap {
 public static void main(String[] args) throws InterruptedException {
   ConcurrentHashMap<String, Integer> stock = new ConcurrentHashMap<>();
   Thread t1 = new Thread(new Runnable() {
     @Override
     public void run() {
       for (int i = 0; i < 1000; i++) {
         stock.merge("article", 1, new java.util.function.BiFunction<Integer,Integer,Integer>() {
           @Override
           public Integer apply(Integer a, Integer b) {
             return a + b;
           }
         });
       }
     }
   });
   Thread t2 = new Thread(new Runnable() {
     @Override
     public void run() {
       for (int i = 0; i < 1000; i++) {
         stock.merge("article", 1, new java.util.function.BiFunction<Integer,Integer,Integer>() {
           @Override
           public Integer apply(Integer a, Integer b) {
             return a + b;
           }
         });
       }
     }
   });
   t1.start();
   t2.start();
   t1.join();
   t2.join();
   System.out.println("Quantité totale: " + stock.get("article"));
 }
}
```

### `ConcurrentHashMap` — mise à jour atomique
```java
import java.util.concurrent.ConcurrentHashMap;

ConcurrentHashMap<String, Integer> stock = new ConcurrentHashMap<>();
stock.putIfAbsent("article", 0);
// Incrément atomique et thread-safe
stock.compute("article", (k, v) -> v == null ? 1 : v + 1);
```

### `Collections.synchronizedMap` — synchronisation à l’itération
```java
import java.util.*;

Map<String, Integer> base = new HashMap<>();
Map<String, Integer> sync = Collections.synchronizedMap(base);

// Écritures/lectures thread-safe (verrou global)
sync.put("a", 1);
sync.put("b", 2);

// Itération : doit être dans un bloc synchronized
synchronized (sync) {
    for (Map.Entry<String, Integer> e : sync.entrySet()) {
        System.out.println(e.getKey() + " => " + e.getValue());
    }
}
```

---
## Voir aussi
- [Documentation `ConcurrentHashMap` officielle](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html)
- [Documentation `Collections.synchronizedMap`](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Collections.html#synchronizedMap(java.util.Map))
---
title: "java.util.concurrent.CopyOnWriteArrayList"
layout: default
parent: "java.util.List"
nav_order: 3
---

# java.util.concurrent.CopyOnWriteArrayList

## Description

`CopyOnWriteArrayList` est une variante *thread-safe* de `ArrayList` qui repose sur le principe de copie lors de l'écriture. Chaque fois qu'une modification (ajout, suppression) est effectuée, un nouveau tableau est créé et les éléments existants sont copiés. Cela garantit que les itérations ne rencontrent jamais de `ConcurrentModificationException`, car elles travaillent sur une version immuable.

## Fonctionnement interne

Les opérations de **lecture** s’effectuent exactement comme avec une `ArrayList` standard : elles sont rapides et sans verrou (**lock**). En revanche, pour **toute opération d’écriture** (ajout, suppression, tri, etc.), `CopyOnWriteArrayList` applique le mécanisme suivant :

1. **Copie complète du tableau interne** afin de préserver l’immutabilité pour les lecteurs.
2. **Application de la modification** (ajout, suppression, réorganisation) sur cette nouvelle copie.
3. **Remplacement atomique** de la référence interne par la version modifiée.

Cette mise à jour est **atomique**, ce qui garantit la cohérence :  
- Les threads lecteurs voient **soit l’ancienne version**, **soit la nouvelle**,  
- **Jamais un état intermédiaire**.

{: .highlight}
> Ce mécanisme conserve la rapidité d'accès de la `ArrayList`, mais rend les écritures coûteuses (**O(n)**), car chaque modification implique une copie complète du tableau.


## Forces et faiblesses

### Forces
- Supporte la concurrence (*thread-safe*) dès la conception (pas besoin de synchronisation externe)

### Faiblesses
- Opérations d'écriture très coûteuses et lentes: copie entière du tableau à chaque fois

## Complexité

| Opération       | Complexité |
|-----------------|------------|
| Accès par index         | O(1) |
| Écriture        | O(n) (copie complète) |


## Quand l'utiliser ?

`CopyOnWriteArrayList` est très efficace pour des scénarios où les lectures sont beaucoup plus fréquentes que les écritures dans un contexte concurrent (*multi-thread*). Cependant, elle est coûteuse en mémoire et en temps pour des modifications fréquentes, car chaque écriture implique une copie complète du tableau.

{: .astuce}
> Pour des scénarios où les écritures sont fréquentes, il est souvent préférable d'utiliser `Collections.synchronizedList(List)` sur une implémentation standard de liste (`LinkedList` ou `ArrayList`). Cette méthode retourne un *wrapper* (patron de conception *Decorator*) qui impose un verrou sur **toutes les opérations**, lecture comme écriture. Ainsi, un seul thread à la fois peut interagir avec la liste, la rendant *thread-safe*.
>
> Lorsqu'il y a beaucoup d'écritures, cette option est généralement moins coûteuse que l'utilisation d'une `CopyOnWriteArrayList`.


## Exemple de code

```java
import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;

public class COWArrayListVsSynchronizedList {
    public static void main(String[] args) {
        
        // Utilisation d'une CopyOnWriteArrayList
        List<String> noms = new CopyOnWriteArrayList<>();
        noms.add("Alice");
        noms.add("Bob");

        for (String nom : noms) {
            System.out.println(nom);
        }

        // Création d'un "wrapper" de synchronisation par-dessus une ArrayList standard
        List<String> autresNoms = new ArrayList<>();
        autresNoms.add("Charlie");
        autresNoms.add("Eve");

        List<String> threadSafeNoms = Collections.synchronizedList(autresNoms);
        for (String nom : autresNoms) {
            System.out.println(nom);
        }

    }
}
```

## Voir aussi

- [Collections.synchronizedList(List<T> list)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Collections.html#synchronizedList(java.util.List))
- [java.util.concurrent.CopyOnWriteArrayList](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/concurrent.CopyOnWriteArrayList.html)

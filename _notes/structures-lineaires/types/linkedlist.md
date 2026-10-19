---
title: "java.util.LinkedList"
layout: default
parent: "java.util.List"
nav_order: 1
published: true
---

# java.util.LinkedList

![Liste chaînée simple](../assets/images/doubly-linked-list.webp)
*Image tirée de geeksforgeeks.org*

## Description

`LinkedList` est une implémentation basée sur une liste chaînée double. Chaque élément est stocké dans un noeud qui contient une référence vers l'élément précédent et suivant. Cette structure permet des insertions et suppressions rapides en tête ou en queue (O(1)), car il suffit de mettre à jour quelques références. Cependant, l'accès par index est coûteux (O(n)), car il faut parcourir la liste depuis le début ou la fin.

## Fonctionnement interne

En interne, `LinkedList` consomme plus de mémoire qu'un tableau car chaque noeud stocke deux références supplémentaires. Elle est idéale pour des scénarios où les insertions et suppressions sont fréquentes, mais les accès aléatoires rares.

---


## Forces et faiblesses

### Forces
- **Insertions et suppressions rapides en tête ou en fin** : opérations en **O(1)** sans décalage des éléments, contrairement à `ArrayList`.
- **Structure chaînée** : adaptée aux listes très dynamiques ou aux réorganisations fréquentes.
- **Navigation bidirectionnelle** : grâce au double chaînage, possibilité de parcourir dans les deux sens.

### Faiblesses
- **Accès par index coûteux** : nécessite un parcours séquentiel (**O(n)**).
- **Moins performant pour un parcours simple** : `ArrayList` est généralement plus rapide pour l’itération pure.
- **Non thread-safe** : nécessite une synchronisation explicite en environnement concurrent.

---

## Quand l’utiliser ?

`LinkedList` est pertinente lorsque la liste subit des **insertions et suppressions fréquentes**, surtout en tête ou en fin, et que l’accès direct par index n’est pas essentiel. Elle convient bien aux **collections dynamiques** dont la structure évolue souvent.

### Cas d’utilisation recommandés
- **Ajouts et suppressions fréquents**, majoritairement en tête ou en fin.
- **Collections très dynamiques** : modifications régulières à des positions variées.
- **Parcours séquentiel privilégié** : accès aléatoire rare.
- **Réorganisations internes** : déplacements d’éléments sans coût de décalage.

---

{: .astuce}
> Pour des insertions/suppressions rapides en tête ou en fin, `LinkedList` est adaptée. Pour des accès rapides par index et des ajouts en fin, préférez `ArrayList`. Pour implémenter des piles ou des files, `LinkedList` peut convenir, mais l’interface **`Deque`** avec une implémentation comme **`ArrayDeque`** est généralement plus performante.



## Complexité

| Opération       | Complexité |
|-----------------|------------|
| Accès par index | O(n) |
| Insertion début/fin  | O(1) |
| Insertion milieu| O(n) |
| Suppression début/fin     | O(1) |
| Suppression milieu     | O(n) |
| Recherche (toujours linéaire) | O(n) |


## Exemple d'utilisation

```java
import java.util.*;

public class DemoLinkedList {
    public static void main(String[] args) {
        List<String> noms = new LinkedList<>();
        noms.add("Alice");
        noms.add("Bob");
        noms.add("Charlie");

        // Avec une LinkedList, il est beaucoup plus efficace d'itérer avec un itérateur ou un for-each que par index.

        // Relativement efficace - O(n)
        for (String nom : noms) {
            System.out.println(nom);
        }

        // Peu efficace - O(n^2), car l'accès par index demande une itération imbriquée
        for (int i = 0; i < noms.size(); i++) {
            System.out.println(noms.get(i));
        }

        Collections.reverse(noms);
        System.out.println("Liste inversée : " + noms);
    }
}
```

## Voir aussi

- [java.util.LinkedList](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/LinkedList.html)

{: .highlight}
> Comme toutes les autres Collections, `LinkedList` fonctionne principalement avec des types génériques. Pour plus d'information sur ce sujet, consultez la page suivante: [Génériques en Java](../notes/generiques-java)

---
title: "java.util.concurrent.ConcurrentLinkedDeque"
parent: "java.util.Deque"
layout: default
nav_order: 2
has_toc: false
published: true
---

# Classe `java.util.ConcurrentLinkedDeque`

## Description
`ConcurrentLinkedDeque` est une implémentation *thread-safe* de `Deque` utilisant une **liste chaînée double**. Elle repose sur des algorithmes non bloquants pour permettre une utilisation en contexte concurrent.

## Fonctionnement interne

Chaque élément est encapsulé dans un noeud contenant des références vers le précédent et le suivant. Contrairement aux structures synchronisées classiques, elle utilise des **algorithmes non bloquants** basés sur des opérations atomiques (*CAS – Compare And Swap*) pour mettre à jour les liens entre les noeuds. Ce mécanisme permet d’éviter les verrous globaux et réduit la contention, ce qui améliore la performance en environnement concurrent. Les opérations d’ajout et de suppression en tête ou en queue sont réalisées en **O(1)**, même avec plusieurs threads, grâce à des mises à jour atomiques des pointeurs. Cette approche garantit la cohérence sans bloquer les autres threads, mais implique une consommation mémoire plus élevée qu’une structure basée sur un tableau.

## Forces et faiblesses 

### Forces

- **Thread-safe** sans verrou global (utilise des algorithmes non bloquants).
- Haute performance en environnement concurrent.
- Pas de contention majeure.

### Faiblesses

- Plus complexe que `ArrayDeque` (surcharge pour la concurrence).
- Consommation mémoire plus élevée.
- Pas adaptée si la concurrence n'est pas nécessaire.

## Quand l'utiliser ?

- Environnements **multi-thread** où plusieurs producteurs/consommateurs accèdent à la même file.
- Systèmes demandant la mise en file, par exemple:
    - Systèmes de **messagerie**.
    - Files de tâches dans des **serveurs applicatifs**.

## Complexité

| Opération | Complexité |
|-----------|------------|
| addFirst / addLast | O(1) amorti |
| removeFirst / removeLast | O(1) amorti |

## Exemple
```java
Deque<String> deque = new ConcurrentLinkedDeque<>();
deque.add("A");
deque.add("B");
System.out.println(deque.poll()); // A
```

### À voir aussi
[Documentation Java ConcurrentLinkedDeque](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/concurrent/ConcurrentLinkedDeque.html)

---
layout: default
title: "Méthode swap(a, b)"
nav_order: 2
has_toc: false
published: true
---
# Exercice: Méthode `swap(a, b)`

## Objectif

Implémenter une méthode `swap(String a, String b)` en Java qui inverse les valeurs de deux variables passées en argument.

## Consignes

1. Créer une classe `Echangeur` avec une méthode `swap(String a, String b)`.
2. Dans la méthode `main`, créer deux variables `a` et `b` avec des valeurs différentes.
3. Appeller la méthode `swap(a, b)` pour tenter d’inverser les deux valeurs.
4. Afficher les valeurs de `a` et `b` avant et après l’appel à `swap`.

## Exemple de code de départ

```java
public class Echangeur {
    public static void swap(String a, String b) {
        String temp = a;
        a = b;
        b = temp;
    }

    public static void main(String[] args) {
        String a = "Bonjour";
        String b = "Salut";

        System.out.println("Avant échange : a = " + a + ", b = " + b);
        swap(a, b);
        System.out.println("Après échange : a = " + a + ", b = " + b);
    }
}
```

## Question de réflexion

Pourquoi les valeurs de `a` et `b` **n’ont pas changé** après l’appel à `swap` ?

## Explication

En Java, les arguments sont toujours passés **par valeur**. Dans le cas des objets comme `String`, cela signifie que **la référence vers l'objet est copiée**. La méthode `swap` modifie les copies locales des références, mais **pas les variables originales** dans `main()`.

## Implémentation

Implémenter une variante qui permettra à la méthode `swap` d'inverser correctement les valeurs `a` et `b`.

### Indices

Pour contourner cette limitation, on peut :

- Passer en argument un **tableau** contenant les deux valeurs.
- Passer en argument un **objet mutable** qui contient les deux valeurs.

## Conclusion

Java passe toujours les arguments **par valeur** à une méthode. Lorsque l'on passe un type primitif (int, double, boolean, etc), on passe **une copie de la valeur primitive**. Lorsque l'on passe un objet, on passe **copie de sa référence**. Cela signifie que lorsqu’on transmet un objet à une méthode, **on ne transmet pas l’objet lui-même**, mais **une copie de son adresse en mémoire**.

Dans les deux cas, si on réassigne la variable qui a été passée en argument, on ne modifiera pas l'objet initial.

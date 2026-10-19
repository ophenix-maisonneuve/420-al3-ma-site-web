---
layout: default
title: "Égalité"
parent: "Concepts de programmation orientée objet"
nav_order: 5
---

# Égalité en Java

## La méthode `equals()`

La méthode `equals()` permet de comparer deux objets Java pour vérifier s'ils sont **logiquement équivalents**. Contrairement à l'opérateur `==`, qui compare les **références mémoire**, `equals()` compare le **contenu** des objets.


{: .warning}

>En Java, les objets sont manipulés via des **références**. Cela signifie que lorsque l'on écrit :
>
>```java
>String a = new String("hello");
>String b = new String("hello");
>System.out.println(a == b);       // false
>System.out.println(a.equals(b));  // true
>```
>
>- `a == b` retourne `false` car `a` et `b` pointent vers **deux objets différents** en mémoire.
>- `a.equals(b)` retourne `true` car le **contenu** des deux objets est identique.
>
>Ainsi, pour comparer le contenu de deux objets, il faut **implémenter correctement `equals()`** dans la classe.

### Exemple d'implémentation de `equals()`

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    Personne autre = (Personne) obj;
    return age == autre.age && nom.equals(autre.nom);
}
```

## La méthode `hashCode()`

La méthode `hashCode()` retourne un entier représentant le **code de hachage** de l'objet. Elle est utilisée dans les structures de données comme `HashMap`, `HashSet`, etc.

### Contrat entre `equals()` et `hashCode()`

- Si deux objets sont **égaux** selon `equals()`, ils doivent avoir le **même `hashCode()`**.
- Cela garantit le bon fonctionnement des collections basées sur le hachage.

### Exemple d'implémentation de `hashCode()`

```java
@Override
public int hashCode() {
    return Objects.hash(nom, age);
}
```

## Exemple complet

```java
public class Personne {
    private String nom;
    private int age;

    public Personne(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Personne autre = (Personne) obj;
        return age == autre.age && nom.equals(autre.nom);
    }

    @Override
    public int hashCode() {
        return Objects.hash(nom, age);
    }
}
```

## Utilisation dans une collection

```java
Set<Personne> personnes = new HashSet<>();
personnes.add(new Personne("Alice", 30));
personnes.add(new Personne("Alice", 30)); // Ne sera pas ajouté si equals() et hashCode() sont bien définis

System.out.println(personnes.size()); // Affiche 1
```

## Bonnes pratiques

- Toujours redéfinir `equals()` et `hashCode()` ensemble.
- Utiliser `Objects.equals()` et `Objects.hash()` pour simplifier le code.
- Ne jamais utiliser `==` pour comparer des objets sauf si tu veux vérifier qu’ils sont **exactement la même instance**.


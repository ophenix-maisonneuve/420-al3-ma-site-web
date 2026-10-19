---
layout: default
title: "Gestion des exceptions"
parent: "Concepts de programmation orientée objet"
nav_order: 9
---

# Gestion des exceptions

La gestion des exceptions en Java permet de traiter les erreurs qui peuvent survenir pendant l'exécution d'un programme. Elle repose sur une hiérarchie de classes et une structure de contrôle dédiée.

---

## Classe de base : `Throwable`

La classe **`Throwable`** est la superclasse de toutes les erreurs et exceptions en Java. Elle possède deux sous-classes principales :

- **`Error`** : représente des erreurs graves que l'application ne devrait pas essayer de gérer (ex. : `OutOfMemoryError`).
- **`Exception`** : représente des conditions que l'application peut vouloir intercepter et gérer.

```java
public class Throwable implements Serializable {
    // ...
}
```

---

## Classe `Error`

Les objets de type `Error` indiquent des problèmes sérieux liés à l'environnement d'exécution (JVM). Ils ne doivent **pas** être interceptés dans la majorité des cas.

Exemples :
- `OutOfMemoryError`
- `StackOverflowError`

---

## Classe `Exception`

Les exceptions sont des événements anormaux que l'on peut **prévoir et gérer** dans le code. Elles se divisent en deux catégories :

### 1. Exceptions vérifiées (*Checked Exceptions*)

- Doivent être **déclarées** dans la signature de la méthode avec `throws`, ou être **capturées** avec un bloc `try/catch`.
- Exemples : `IOException`, `SQLException`

```java
public void lireFichier(String nomFichier) throws IOException {
    FileReader fr = new FileReader(nomFichier);
}
```

### 2. Exceptions non vérifiées (*Unchecked Exceptions*)

- Héritent de `RuntimeException`
- Ne nécessitent **pas** de déclaration explicite
- Exemples : `NullPointerException`, `ArrayIndexOutOfBoundsException`

```java
int[] tableau = new int[3];
System.out.println(tableau[5]); // Provoque ArrayIndexOutOfBoundsException
```

---

## Structure `try` / `catch` / `finally`

La structure `try/catch/finally` permet de gérer les exceptions de manière contrôlée.

```java
try {
    // Code susceptible de générer une exception
    int resultat = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Erreur : division par zéro.");
} finally {
    System.out.println("Bloc finally exécuté dans tous les cas.");
}
```

- **`try`** : contient le code à risque
- **`catch`** : capture et gère l'exception
- **`finally`** : s'exécute toujours, qu'une exception soit levée ou non (utile pour libérer des ressources)

---

## Exceptions multiples

Il est possible qu'une section de code puisse générer plusieurs exceptions. Lorsque c'est le cas, il existe différentes stratégies pour gérer ces exceptions, selon la manière dont on veut les traiter.

### Clauses `catch` distinctes

C'est la méthode la plus conventionnelle, qui existe depuis la création de Java. Elle permet d'attraper chacun des types d'exception générées et de traiter chaque type individuellement.

#### Exemple

```java
try {
    int nombre = Integer.parseInt("abc"); // lance NumberFormatException
    int resultat = 10 / 0; // lance ArithmeticException
} catch (NumberFormatException e) {
    System.out.println("Erreur de format : " + e.getMessage());
} catch (ArithmeticException e) {
    System.out.println("Erreur arithmétique : " + e.getMessage());
} finally {
    System.out.println("Bloc finally exécuté dans tous les cas.");
}
```

### Multi-catch

Depuis Java 7, il est possible de capturer plusieurs types d'exceptions dans un seul bloc `catch` en les séparant par le symbole `|`. Lorsque l'on désire traiter plusieurs types d'exceptions de la même manière, cela permet de **réduire la duplication de code**.

#### Exemple :

```java
try {
    int nombre = Integer.parseInt("abc"); // lance NumberFormatException
    int resultat = 10 / 0; // lance ArithmeticException
} catch (NumberFormatException | ArithmeticException e) {
    System.out.println("Une exception est survenue : " + e.getClass().getSimpleName());
} finally {
    System.out.println("Bloc finally exécuté dans tous les cas");
}
```

### Règles importantes :
- Les types d'exceptions doivent **hériter de la même classe de base** (souvent `Exception`).
- Les exceptions **ne doivent pas avoir de relation d’héritage entre elles**, sinon le compilateur générera une erreur.
- La variable `e` est **effectivement finale** : tu ne peux pas lui réassigner une autre valeur dans le bloc `catch`.

---

## Bonnes pratiques
- Ne pas intercepter des `Error`, sauf cas très particuliers
- Toujours capturer les exceptions spécifiques plutôt que `Exception` générique
- Utiliser `finally` pour fermer les ressources (fichiers, connexions, etc.)
- Éviter d'utiliser les exceptions pour contrôler le flux normal du programme

---

## Références
- [Learn Java - Exceptions](https://dev.java/learn/exceptions/)
- [Java SE API - Throwable](https://docs.oracle.com/javase/25/docs/api/java/lang/Throwable.html)


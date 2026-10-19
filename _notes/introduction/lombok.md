---
layout: default
title: "Librairie Lombok"
parent: "Introduction à Java et ses outils"
nav_order: 5
published: true
---

# Librairie Lombok

## Qu'est-ce que Lombok ?

[Lombok](https://projectlombok.org/) est une bibliothèque Java qui permet de **réduire le code *boilerplate*** (code répétitif et souvent verbeux) en utilisant des **annotations**. Elle s'intègre facilement dans les projets Java via Maven ou Gradle et fonctionne avec la plupart des IDE modernes comme IntelliJ IDEA, Visual Studio Code et Eclipse

---

## Pourquoi utiliser Lombok ?

En Java traditionnel (sans Lombok), les développeurs écrivent couramment des méthodes assez répétitives, comme :

- `getters` et `setters`
- `constructeurs`
- `toString()`
- `equals()` et `hashCode()`

De plus, les **exceptions gérées** (*checked exceptions*) entraînent parfois beaucoup de code redondant simplement pour propager une exception jusqu'au niveau où l'on veut la gérer.

Toutes ces situations peuvent bénéficier de l'utilisation de Lombok, qui automatise ces tâches grâce à des annotations simples.

{: .warning}

>Bien que **Lombok** apporte plusieurs avantages, il faut aussi être conscient de quelques inconvénients possibles:
>- **Dépendance à une bibliothèque externe** : peut poser problème dans certains environnements de build.
>- **Moins de visibilité** sur le code généré automatiquement.
>- **Debugging plus complexe** : les méthodes générées ne sont pas visibles dans le code source.

---

## Annotations principales

### `@Getter` / `@Setter`
Génère automatiquement les accesseurs et mutateurs.
```java
@Getter @Setter
public class Student {
    private String name;
    private int age;
}
```

### `@NonNull`
Ajoute une vérification de nullité.
```java
public class Account {
    public Account(@NonNull String id) {
        this.id = id;
    }
    private String id;
}
```

### `@NoArgsConstructor` / `@AllArgsConstructor`
Génère le constructeurs par défaut (vide) et/ou un constructeur permettant de définir tous les champs.
```java
@NoArgsConstructor
@AllArgsConstructor
public class Course {
    private String name;
    private int credits;
}
```

### `@RequiredArgsConstructor`
Génère un constructeur avec les champs `final` ou annotés `@NonNull`.
```java
@RequiredArgsConstructor
public class User {
    private final String username;
    @NonNull private String email;
}
```

### `@ToString`
Génère une méthode `toString()`.
```java
@ToString
public class Point {
    private int x;
    private int y;
}
```

### `@EqualsAndHashCode`
Génère les méthodes `equals()` et `hashCode()`.
```java
@EqualsAndHashCode
public class Employee {
    private int id;
    private String name;
}
```

### `@Data`
Combine `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, etc.
```java
@Data
public class Book {
    private String title;
    private String author;
}
```

### `@Builder`
Implémente le pattern Builder.
```java
@Builder
public class Car {
    private String brand;
    private int year;
}
```

### `@SneakyThrows`
Permet de lancer des exceptions sans les déclarer.
```java
@SneakyThrows
public void readFile(String path) {
    Files.readAllLines(Paths.get(path));
}
```

---

## Installation avec Maven

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.42</version>
</dependency>
```

---

## Ressources utiles

- [Documentation officielle](https://projectlombok.org/features/all)

---

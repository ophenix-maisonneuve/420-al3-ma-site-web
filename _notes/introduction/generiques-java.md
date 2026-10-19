---
title: "Génériques en Java"
parent: "Introduction à Java et ses outils"
layout: default
nav_order: 4
published: true
---

# Les génériques en Java

## Introduction
Les **génériques** ont été introduits dans **Java 5** (2004). Avant cette version, les collections comme `List` ou `Map` stockaient des objets de type `Object`, ce qui obligeait à faire des **casts explicites** et pouvait provoquer des erreurs à l’exécution.

Exemple avant Java 5 :
```java
List liste = new ArrayList();
liste.add("Bonjour");
String texte = (String) liste.get(0); // Cast obligatoire
```

Avec les génériques (Java 5 et plus) :
```java
List<String> liste = new ArrayList<>();
liste.add("Bonjour");
String texte = liste.get(0); // Pas de cast, sécurité accrue
```

**Pourquoi les génériques ?**
- **Sécurité de type** : les erreurs sont détectées à la compilation.
- **Lisibilité** : pas besoin de casts explicites.
- **Réutilisabilité** : un même code peut fonctionner avec différents types.

---

## Fonctionnement général
Un **générique** permet de définir une classe, une interface ou une méthode avec un **paramètre de type**. Ce paramètre est remplacé par un type concret lors de l’utilisation.

Exemple simple :
```java
class Boite<T> {
    private T contenu;

    public void setContenu(T contenu) {
        this.contenu = contenu;
    }

    public T getContenu() {
        return contenu;
    }
}

// Utilisation
Boite<String> boiteTexte = new Boite<>();
boiteTexte.setContenu("Bonjour");
System.out.println(boiteTexte.getContenu());
```

Ici, `T` est un **paramètre de type**. Lorsqu’on crée `Boite<String>`, `T` devient `String`.

---

## Autres exemples
### Utilisation d'une liste typée
```java
List<Integer> nombres = new ArrayList<>();
nombres.add(10);
nombres.add(20);

for (Integer n : nombres) {
    System.out.println(n);
}
```

### Définition et utilisation d'une méthode générique
```java
public static <T> void afficher(T element) {
    System.out.println(element);
}

afficher("Bonjour");
afficher(123);
```

---

## Utilisation d’un type *raw* (non typé)
Un **type *raw*** est une utilisation d’une classe générique **sans préciser le type**. Elle est équivalente à la manière dont les collections étaient utilisées avant l'introduction des génériques avec Java 5.
```java
List liste = new ArrayList(); // Type raw
liste.add("Texte");
liste.add(123); // Autorisé !
```

Conséquences :
- **Pas de vérification à la compilation**.
- Risque d’erreurs à l’exécution :
```java
String s = (String) liste.get(1); // ClassCastException
```

{: .warning}
Il est préférable d'éviter les types *raw*, sauf pour compatibilité avec du code ancien.

---

## *type erasure*
En Java, les **génériques n’existent qu’à la compilation**. Le compilateur vérifie les types, puis **supprime les informations de type** (*type erasure*) avant d’exécuter l'application.

Exemple :
```java
List<String> liste = new ArrayList<>();
```
Après compilation, cela devient :
```java
List liste = new ArrayList(); // Sans information de type
```

**Pourquoi ?**
- Compatibilité avec les anciennes versions de Java.
- Les informations de type sont utilisées uniquement pour la sécurité à la compilation.

---

## Limites des génériques en Java
Bien que les génériques soient puissants, ils ont certaines **restrictions importantes** :

### Pas de types primitifs
Les génériques ne peuvent pas utiliser de types primitifs (`int`, `double`, etc.). Il faut utiliser leurs classes enveloppes (`Integer`, `Double`, etc.) :
```java
List<int> nombres; // Interdit
List<Integer> nombres; // Correct
```

### Pas d’instanciation directe du type générique
Impossible de créer une instance du type paramétré :
```java
class Boite<T> {
    private T contenu;
    public Boite() {
        // contenu = new T(); // Interdit
    }
}
```

### Pas de surcharge basée sur les types génériques
Deux méthodes qui diffèrent uniquement par le type générique ne peuvent pas coexister :
```java
void methode(List<String> liste) {}
void methode(List<Integer> liste) {} // Erreur : même signature après type erasure
```

### Pas d’accès au type à l’exécution
À cause du **type erasure**, il est impossible de vérifier le type paramétré à l’exécution :
```java
if (element instanceof T) { } // Interdit
```

---

## En résumé
- Les génériques apportent **sécurité**, **lisibilité** et **réutilisabilité**.
- Toujours **spécifier le type** pour éviter les erreurs.
- Le **type erasure** explique pourquoi il n’y a pas de surcharge basée sur les types génériques (ex. : impossible d’avoir deux méthodes `methode(List<String>)` et `methode(List<Integer>)`).
- Les génériques sont une fonctionnalité **de compilation**, pas d’exécution.

---

## Ressources externes
Pour aller plus loin :
- [API Java SE 25 – Documentation officielle](https://docs.oracle.com/en/java/javase/25/docs/api/index.html)
- [Tutoriel sur les génériques](https://dev.java/learn/generics/)

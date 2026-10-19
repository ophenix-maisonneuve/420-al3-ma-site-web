---
layout: default
title: "ISP - Ségrégation des interfaces"
parent: "Principes SOLID"
nav_order: 4
published: true
---

# ISP — Ségrégation des interfaces (*Interface Segregation Principle*)

Il vaut mieux **plusieurs petites interfaces spécifiques** qu’une **grande interface** généraliste.

## Pourquoi ?
Les interfaces de type *couteau suisse* (c'est-à-dire des interfaces qui tentent de tout définir) forcent les implémenteurs à **coder des méthodes inutiles** (souvent vides), multiplient les **dépendances** et **complexifient** les tests inutilement. Elles forcent aussi le code à exposer **toutes** les capacités d'un objet d'un coup (approche tout ou rien). Dans l'exemple ci-bas, il devient très difficile (voire impossible) d'exposer seulement la capacité d'impression de l'appareil multifonction à un appelant.

## Définition
L’**ISP** force à **modéliser des capacités** ciblées (imprimer, numériser, faxer) et à **composer** les besoins d’un client avec **plusieurs petites interfaces**. On évite ainsi les interfaces gonflées qui rassemblent des responsabilités **artificiellement** ; les implémenteurs implémentent **seulement** ce qu’ils utilisent, ce qui **réduit** le couplage, **clarifie** les contrats et **accélère** les tests. En combinaison avec le **SRP** (pour garder une seule raison de changer) et le **DIP** (pour dépendre d’abstractions), l’ISP rend le système **modulaire** et **évolutif**.

## Exemple qui ne respecte pas ISP
```java
public interface AppareilMultifonction {
    void imprimer(String contenu);
    void scanner();
    void faxer(String numero, String contenu);
}

public class Imprimante implements AppareilMultifonction {
    public void imprimer(String contenu) {
        System.out.println("Impression: " + contenu);
    }

    public void scanner() {
        throw new UnsupportedOperationException("Scan non supporté.")
    }

    public void faxer(String numero, String contenu) {
        throw new UnsupportedOperationException("Fax non supporté.");
    }
}
```

{: .warning}
> Ici, l’interface **oblige** à implémenter des méthodes **inutiles** pour `Imprimante`. On **couple** le code à des capacités qui ne sont pas requises et on **alourdit** la classe pour rien.

## Exemple corrigé conforme à ISP
```java
public interface Imprimante {
    void imprimer(String contenu);
}

public interface Scanner {
    void scanner();
}
public interface Telecopieur {
    void faxer(String numero, String contenu);
}

public class ImprimanteLaser implements Imprimante {
    public void imprimer(String contenu) { System.out.println("Impression: " + contenu); }
}cd 

/**
 * Appareil qui fait tout, sauf votre lunch... 
 */
public class AppareilMultiFonction implements Imprimante, Scanner, Telecopieur {
    public void imprimer(String contenu) {
        System.out.println("Impression pro: " + contenu);
    }

    public void scanner() {
        System.out.println("Scan pro.");
    }

    public void faxer(String numero, String contenu) {
        System.out.println("Fax vers " + numero + " : " + contenu);
    }
}
```

{: .highlight}
> Les interfaces sont maintenant **séparées** par **capacité**. Chaque implémenteur implémente **seulement** ce dont il a besoin, ce qui **réduit** le couplage et **simplifie** les tests et l’évolution.

## Liens utiles
- [https://en.wikipedia.org/wiki/Interface_segregation_principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- [https://stackify.com/interface-segregation-principle/](https://stackify.com/interface-segregation-principle/)

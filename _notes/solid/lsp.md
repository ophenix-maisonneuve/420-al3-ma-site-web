---
layout: default
title: "LSP - Substitution de Liskov"
parent: "Principes SOLID"
nav_order: 3
published: true
---

# LSP — Substitution de Liskov (*Liskov Substitution Principle*)

Les **types dérivés** doivent pouvoir **se substituer** aux types de base **sans altérer** la logique attendue.

## Pourquoi ?
Une hiérarchie **mal conçue** peut briser les comportements attendus du parent ; le code qui dépend du type de base se **comporte différemment** lorsqu'utilisé avec une implémentation particulière (une sous-classe), entraînant des **erreurs** inattendues.

## Définition
Le **LSP** impose une constance dans le comportement des **sous‑types** : un sous‑type doit respecter les **pré‑conditions**, **post‑conditions** et **invariants** du type parent (ou le contrat implicite de l'interface). En héritage, toute **restriction** ou **renforcement** mal aligné peut rompre la substituabilité (voir l'exemple *Rectangle/Carré* ci-bas). Quand les invariants **diffèrent**, il vaut mieux recourir à la **composition** ou à des **types indépendants**, assortis d’une **abstraction commune** (classe abstraite ou interface). Cette rigueur évite des tests spécifiques aux enfants ou des *casts* explicites et **préserve** l’**OCP** en permettant d’introduire de nouveaux types sans changer le code client.

## Exemple qui ne respecte pas LSP
```java
public class Rectangle {
    private int largeur;
    private int hauteur;

    public int getLargeur() { 
        return this.largeur; 
    }

    public void setLargeur(int largeur) {
        this.largeur = largeur;
    }

    public int getHauteur() {
        return this.hauteur;
    }

    public void setHauteur(int hauteur) {
        this.hauteur = hauteur;
    }

    public int aire() {
        return this.largeur * this.hauteur;
    }
}

public class Carre extends Rectangle {
    @Override
    public void setLargeur(int largeur) {
        super.setLargeur(largeur);
        super.setHauteur(largeur);
    }
    @Override
    public void setHauteur(int hauteur) {
        super.setHauteur(hauteur);
        super.setLargeur(hauteur);
    }
}

public class TestRectangle {
    public int calculer(Rectangle r) {
        r.setLargeur(5);
        r.setHauteur(4);
        return r.aire();
    }
}
```

{: .warning}
> Ici, `Carre` **modifie l’invariant** attendu par `Rectangle` (largeur indépendante de la hauteur). Le client qui manipule `Rectangle` **n’obtient plus** le comportement attendu (aire 20) et la **substituabilité** est enfreinte.

## Exemple corrigé conforme au LSP
```java
public interface Forme {
    int aire();
}

public class Rectangle implements Forme {
    private int largeur;
    private int hauteur;
    
    public Rectangle(int largeur, int hauteur) {
        this.largeur = largeur; this.hauteur = hauteur;
    }

    public int getLargeur() {
        return this.largeur;
    }

    public int getHauteur() {
        return this.hauteur;
    }

    public int aire() {
        return this.largeur * this.hauteur;
    }
}

public class Carre implements Forme {
    private int cote;
    public Carre(int cote) {
        this.cote = cote;
    }

    public int getCote() {
        return this.cote;
    }

    public int aire() {
        return this.cote * this.cote;
    }
}

public class CalculAire {
    public int aireForme(Forme forme) {
        return forme.aire();
    }
}
```

{: .highlight}
> `Rectangle` et `Carre` deviennent **deux types distincts** partageant l’abstraction `Forme`. Chacun **préserve son invariant** et peut être **substitué** via `Forme` sans causer de mauvaises surprises au code qui utilise l'interface de base `Forme`.

## Liens utiles
- [https://en.wikipedia.org/wiki/Liskov_substitution_principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
- [https://www.baeldung.com/cs/liskov-substitution-principle](https://www.baeldung.com/cs/liskov-substitution-principle)

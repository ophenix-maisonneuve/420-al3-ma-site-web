---
layout: default
title: "Builder"
parent: "Patrons de conception"
nav_order: 7
has_toc: false
published: true
---

# Exercice : Café personnalisé
*Parce qu'on a tous besoin d'un latté skinny sans gras, double shot extra‑long, mousse d’avoine micro‑texturée, 2 pompes de vanille artisanale avec dessin de phoque perplexe et une pincée de cannelle*.

Le nouveau café du coin a entendu parler de vos exploits en développement par des amis qui sont propriétaires d'une agence de voyages. Ils vous contactent afin de raffiner un peu l'application qu'ils utilisent afin de gérer les commandes de café de leurs clients, qui deviennent de plus en plus complexes.

## Objectifs

- Comprendre le fonctionnement du patron de conception *Builder*
- Reconnaître les cas d'utilisation propices pour le patron *Builder*

## Contexte

À partir d'une classe fournie, suivre les étapes proposées afin de comprendre les enjeux liés à l'utilisation de constructeurs complexes et les avantages d'utiliser le patron de conception *Builder* pour de tels cas.

## Étapes préparatoires

### 1. Copiez la classe de départ suivante dans votre IDE ou environnement de choix
```java
public class Cafe {
    private String type;
    private String taille;

    public Cafe(String type, String taille) {
        this.type = type;
        this.taille = taille;
    }

    @Override
    public String toString() {
        return "Cafe{type='%s', taille='%s'}".formatted(this.type, this.taille);
    }

    public static void main(String[] args) {
        List<Cafe> commandes = new ArrayList<>();
        commandes.add(new Cafe("latte", "petit"));
        commandes.add(new Cafe("cappuccino", "grand"));

        commandes.forEach(System.out::println);
    }
}
```

## Questions

### 1. Ajoutez les options `lait` et `sirop`
On vous demande maintenant d'ajouter des options pour le type de lait et le type de sirop. Votre premier réflexe est de créer les deux nouveaux constructeurs suivants:
```java
public Cafe(String type, String taille, String lait) {
   this(type, taille);
   this.lait = lait;
}

public Cafe(String type, String taille, String lait, String sirop) {
   this(type, taille, lait);
   this.sirop = sirop;
}
```
- Ajoutez les constructeurs ci-haut dans votre code en modifiant la classe au besoin.
- Utilisez maintenant vos nouveaux constructeurs dans la méthode `main`.
- Que remarquez-vous quant à la lisibilité / facilité d'utilisation de votre code ?


### 2. Ajoutez maintenant les options `nombreShots`, `temperature` et `sucre`
Horreur! On vous demande maintenant d'ajouter des options permettant aux clients de choisir le nombre de *shots* d'espresso, la température du breuvage ainsi que le nombre de sucres. 

- Quel est le risque de continuer à ajouter des constructeurs ?
- Quel est le risque de rendre l'objet `Cafe` mutable et d'utiliser les `setters` au lieu des constructeurs ?
- Comment le patron `Builder` peut-il vous aider à résoudre ces problèmes ?


### 3. Utilisez le patron `Builder` afin d'améliorer la construction du `Cafe`
- Créez d'abord une classe imbriquée appelée `Builder` dans votre classe `Cafe`
   - Ajoutez les différentes propriétés de votre café comme champs dans le `Builder`
   - Lorsque cela fait du sens, initialisez les champs avec une valeur par défaut.
      - *Cela permet à l'appelant de ne pas avoir à appeler toutes les méthodes de construction; seulement celles qui changent les valeurs par défaut*
   - Créez les `getters/setters` pour les champs.

- Dans la classe `Cafe`...
   - Supprimez (ou mettez en commentaires) tous les constructeurs télescopiques que vous avez créés précédemment.
   - Créez un nouveau constructeur **privé** qui ne prend que le `Builder` en paramètre.
   - Dans ce constructeur, assignez les valeurs des champs du `Builder` aux champs correspondants de la classe `Cafe`

- Dans la méthode `main`
   - Utilisez votre `Builder` pour construire l'objet plutôt que les constructeurs qui avaient été créés précédemment.



### 4. Bonus: `Builder` avec interface chaînée (*fluent interface*)
Une variante courante et plus moderne du patron de conception `Builder` consiste à l'utiliser avec une interface chaînée, ce qui rend le code encore plus lisible. Voici un exemple du `Builder` des notes de cours (construction de maison) qui utilise le principe du chaînage :

```java
public class Maison {
    private final int fenetres;
    private final int portes;
    private final boolean garage;

    private Maison(Builder builder) {
        this.fenetres = builder.getFenetres();
        this.portes = builder.getPortes();
        this.garage = builder.hasGarage();
    }

    public int getFenetres() {
        return this.fenetres;
    }

    public int getPortes() {
        return this.portes;
    }

    public boolean hasGarage() {
        return this.garage;
    }


    public int getFenetres() {
        return this.fenetres;
    }

    public int getPortes() {
        return this.portes;
    }

    public boolean hasGarage() {
        return this.garage;
    }


    public static class Builder {

        // Valeurs par défaut lorsque cela fait du sens.
        // Cela permet de ne pas avoir à invoquer toutes les méthodes du builder.
        private int fenetres = 4;
        private int portes = 2;
        private boolean garage = false;

        public Builder withFenetres(int nb) {
            this.fenetres = nb;
        }

        public Builder withPortes(int nb) {
            this.portes = nb;
        }

        public Builder withGarage(boolean garage) {
            this.garage = garage;
        }

        public Maison build() {
            // validations
            if (this.portes < 1) {
                throw new IllegalArgumentException("Une maison doit avoir au moins une porte.");
            }
            if (this.fenetres < 1) {
                throw new IllegalArgumentException("Une maison doit avoir au moins une fenêtre.")
            }

            return new Maison(this);
        }
    }

    public static void main(String[] args) {
        Maison maison = new Builder().withFenetres(10).withPortes(2).withGarage(false).build();
        System.out.println("Maison construite: " + maison.getFenetres() + " fenêtres");
    }
}

```

- Modifiez votre implémentation de `Builder` afin qu'il utilise le chaînage.
- Que constatez-vous quant à la lisibilité de votre code ?




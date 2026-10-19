---
layout: default
title: "Corrigé - Builder"
parent: "Builder"
nav_order: 1
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


## Corrigé

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
   - ***On remarque que le code devient moins lisible à mesure que l'on ajoute des paramètres aux constructeurs.***


### 2. Ajoutez maintenant les options `nombreShots`, `temperature` et `sucre`
Horreur! On vous demande maintenant d'ajouter des options permettant aux clients de choisir le nombre de *shots* d'espresso, la température du breuvage ainsi que le nombre de sucres. 

- Quel est le risque de continuer à ajouter des constructeurs ?
   - ***La lisibilité et la facilité d'utilisation de notre code seront affectées.***
- Quel est le risque de rendre l'objet `Cafe` mutable et d'utiliser les `setters` au lieu des constructeurs ?
   - ***On dévie de l'intention originale : l'objet `Café` n'a pas été conçu pour être mutable; on le rendrait mutable simplement pour accommoder notre besoin de lisibilité.***
- Comment le patron `Builder` peut-il vous aider à résoudre ces problèmes ?
   - ***Le patron `Builder` nous permettra de conserver l'immuabilité de notre objet `Cafe` tout en améliorant la lisibilité et la facilité d'utilisation de notre code.***


### 3. Utilisez le patron `Builder` afin d'améliorer la construction du `Cafe`
```java
public class Cafe {
    private String type;
    private String taille;
    private String lait;
    private String sirop;

    private int nombreShots;
    private int temperature;
    private int sucres;

    private Cafe(Builder builder) {
        this.type = builder.getType();
        this.taille = builder.getTaille();
        this.lait = builder.getLait();
        this.sirop = builder.getSirop();

        this.nombreShots = builder.getNombreShots();
        this.temperature = builder.getTemperature();
        this.sucres = builder.getSucres();
    }

    @Override
    public String toString() {
        return "Cafe{type='%s', taille='%s', lait='%s', sirop='%s', nombreShots='%d', temperature='%d', sucres='%d'}"
                .formatted(this.type, this.taille, this.lait, this.sirop, this.nombreShots, this.temperature,
                        this.sucres);
    }

    public static class Builder {
        private String type;
        private String taille = "moyen";
        private String lait = "vache";
        private String sirop = null;

        private int nombreShots = 1;
        private int temperature = 180;
        private int sucres = 0;

        public String getType() {
            return type;
        }

        public void setType(String type) {
            this.type = type;
        }

        public String getTaille() {
            return taille;
        }

        public void setTaille(String taille) {
            this.taille = taille;
        }

        public String getLait() {
            return lait;
        }

        public void setLait(String lait) {
            this.lait = lait;
        }

        public String getSirop() {
            return sirop;
        }

        public void setSirop(String sirop) {
            this.sirop = sirop;
        }

        public int getNombreShots() {
            return nombreShots;
        }

        public void setNombreShots(int nombreShots) {
            this.nombreShots = nombreShots;
        }

        public int getTemperature() {
            return temperature;
        }

        public void setTemperature(int temperature) {
            this.temperature = temperature;
        }

        public int getSucres() {
            return sucres;
        }

        public void setSucres(int sucres) {
            this.sucres = sucres;
        }

        public Cafe build() {
            return new Cafe(this);
        }
    }

    public static void main(String[] args) {
        List<Cafe> commandes = new ArrayList<>();

        Builder builder = new Builder();
        builder.setType("latte");
        builder.setLait("avoine");
        builder.setNombreShots(2);
        builder.setSirop("vanille");
        commandes.add(builder.build());

        commandes.forEach(System.out::println);
    }
}
```



### 4. Bonus: `Builder` avec interface chaînée (*fluent interface*)
Une variante courante et plus moderne du patron de conception `Builder` consiste à l'utiliser avec une interface chaînée, ce qui rend le code encore plus lisible. Voici un exemple du `Builder` des notes de cours (construction de maison) qui utilise le principe du chaînage :

- Modifiez votre implémentation de `Builder` afin qu'il utilise le chaînage.
- Que constatez-vous quant à la lisibilité de votre code ?

```java
public class Cafe {
    private String type;
    private String taille;
    private String lait;
    private String sirop;

    private int nombreShots;
    private int temperature;
    private int sucres;

    private Cafe(BuilderChaine builder) {
        this.type = builder.getType();
        this.taille = builder.getTaille();
        this.lait = builder.getLait();
        this.sirop = builder.getSirop();

        this.nombreShots = builder.getNombreShots();
        this.temperature = builder.getTemperature();
        this.sucres = builder.getSucres();
    }

    @Override
    public String toString() {
        return "Cafe{type='%s', taille='%s', lait='%s', sirop='%s', nombreShots='%d', temperature='%d', sucres='%d'}"
                .formatted(this.type, this.taille, this.lait, this.sirop, this.nombreShots, this.temperature,
                        this.sucres);
    }

    public static class BuilderChaine {
        private String type;
        private String taille = "moyen";
        private String lait = "vache";
        private String sirop = null;

        private int nombreShots = 1;
        private int temperature = 180;
        private int sucres = 0;

        public String getType() {
            return type;
        }

        public BuilderChaine withType(String type) {
            this.type = type;
            return this;
        }

        public String getTaille() {
            return taille;
        }

        public BuilderChaine withTaille(String taille) {
            this.taille = taille;
            return this;
        }

        public String getLait() {
            return lait;
        }

        public BuilderChaine withLait(String lait) {
            this.lait = lait;
            return this;
        }

        public String getSirop() {
            return sirop;
        }

        public BuilderChaine withSirop(String sirop) {
            this.sirop = sirop;
            return this;
        }

        public int getNombreShots() {
            return nombreShots;
        }

        public BuilderChaine withNombreShots(int nombreShots) {
            this.nombreShots = nombreShots;
            return this;
        }

        public int getTemperature() {
            return temperature;
        }

        public BuilderChaine withTemperature(int temperature) {
            this.temperature = temperature;
            return this;
        }

        public int getSucres() {
            return sucres;
        }

        public BuilderChaine withSucres(int sucres) {
            this.sucres = sucres;
            return this;
        }

        public Cafe build() {
            return new Cafe(this);
        }
    }

    public static void main(String[] args) {
        List<Cafe> commandes = new ArrayList<>();

        BuilderChaine builder = new BuilderChaine();
        Cafe cafe = builder.withType("latte").withLait("avoine").withNombreShots(2).withSirop("vanille").build();
        commandes.add(cafe);

        commandes.forEach(System.out::println);
    }
}

```




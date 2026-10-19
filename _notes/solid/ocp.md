---
layout: default
title: "OCP - Ouvert/Fermé"
parent: "Principes SOLID"
nav_order: 2
published: true
---

# OCP — Ouvert/Fermé (*Open/Closed Principle*)

Les entités doivent être **ouvertes à l’extension**, mais **fermées à la modification**.

## Pourquoi ?
Les longues chaînes de **conditions** (`if`, `else if`, `else`, `switch`/`case`, etc) obligent à **modifier** du code existant pour chaque nouveau cas, ce qui **fragilise** une base validée et accroît le **risque de régression** dès qu'un changement est requis.

## Définition
L’**OCP** privilégie la stabilité du code existant en contrôlant la manière dont les nouvelles fonctionnalités sont ajoutées. On conçoit une **abstraction stable** (interface, classe abstraite) que l’on **étend** par de **nouvelles implémentations** plutôt que par la modification du code client. Le patron **Stratégie** est un bon exemple de respect du principe **OCP**: on fournit à la classe consommatrice la ou les classes fonctionnelles à utiliser, qu’on peut ensuite **substituer** sans toucher à la classe elle‑même. Cette approche **réduit** les *switch* ou *if/else* qui s’allongent, **protège** le code testé et **facilite** l’ajout de fonctionnalités. Couplé au **DIP** (dépendre d’interfaces plutôt que d'implémentations) et au **LSP** (garantir la substituabilité), l’OCP rend l’application **extensible** et **robuste**.

## Exemple qui ne respecte pas OCP
```java
public class CalculateurRemise {
    public double calculerRemise(String typeClient, double montant) {
        if (typeClient.equals("STANDARD")) {
            return montant * 0.05;
        } else if (typeClient.equals("VIP")) {
            return montant * 0.15;
        } else if (typeClient.equals("ETUDIANT")) {
            return montant * 0.10;
        } else {
            return 0.0;
        }
    }
}
```

{: .warning}
> Ici, ajouter une nouvelle catégorie (*PRO*, *ASSOCIATION*, etc.) impose de **modifier** `CalculateurRemise`. Le code est **fermé** à l’extension et **ouvert** aux modifications (et donc aux régressions), ce qui est l'inverse de ce que l'on souhaite.

## Exemple corrigé conforme au OCP
```java
public interface Remise {
    double calculerPour(double montant);
}

public class RemiseStandard implements Remise {
    public double calculerPour(double montant) {
        return montant * 0.05;
    }
}

public class RemiseVip implements Remise {
    public double calculerPour(double montant) {
        return montant * 0.15;
    }
}

public class RemiseEtudiant implements Remise {
    public double calculerPour(double montant) {
        return montant * 0.10;
    }
}

// Le calculateur ne dépend que de l'interface et reçoit l'implémentation par constructeur
public class CalculateurRemise {
    private Remise remise;

    public CalculateurRemise(Remise remise) {
        this.remise = remise;
    }

    public double appliquer(double montant) {
        return this.remise.calculerPour(montant);
    }
}
```

{: .highlight}
> `CalculateurRemise` **ne change plus** : on utilise un sous-type de `Remise` (ex. `RemisePro`) pour étendre le comportement **sans modifier** le code existant. Le code est donc **ouvert** à l'extension et **fermé** aux modifications, respectant ainsi le principe **OCP**.

## Liens utiles
- [https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)
- [https://martinfowler.com/ieeeSoftware/protectedVariation.pdf](https://martinfowler.com/ieeeSoftware/protectedVariation.pdf)

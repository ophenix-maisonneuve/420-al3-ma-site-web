---
layout: default
title: Strategy
parent: Patrons comportementaux
nav_order: 1
published: true
---

## Description
Strategy définit une famille d’algorithmes, encapsule chacun d’eux et les rend interchangeables au sein du contexte qui les utilise. Les différents comportements sont interchangeables dynamiquement, c'est-à-dire que l'on ne doit pas avoir à redémarrer l'application pour changer de comportement.

## Quand l'utiliser ?
- Il existe plusieurs variantes d’un comportement et que l'on désire les permuter dynamiquement.
- Pour éviter des instructions conditionnelles complexes dispersées (`if/else` ou `switch` sur un critère pour définir un comportement précis).

## Avantages
- Substitution dynamique des comportements (pendant l'exécution).
- Respect du principe ouvert/fermé (***OCP***) et meilleure isolation des comportements.

## Inconvénients
- Augmente le nombre de classes.
- Le client doit connaître les stratégies disponibles afin de les fournir au contexte.

{: .highlight}
> Dans la plupart des ouvrages sur le patron *Strategy*, on n'a qu'une seule méthode dans l'interface de la stratégie. Il ne s'agit pas d'une obligation; une stratégie pourrait en théorie avoir plusieurs méthodes. Il faut cependant s'assurer que ces méthodes sont intimement liées (voire indissociables), de façon à préserver l'esprit du patron *Strategy*, qui préconise des algorithmes simples.

---

## Exemple

### Diagramme de classes
```mermaid
classDiagram
class CompressionStrategy {
  <<interface>>
  +compresser(fichier: String): void
}

class ZipCompressionStrategy {
}

class RarCompressionStrategy {
}

class SevenZCompressionStrategy {
}

class CompressionContext {
  -strategy: CompressionStrategy
  +setStrategy(strategy: CompressionStrategy): void
  +compresserFichier(fichier: String): void
}

CompressionStrategy <|.. ZipCompressionStrategy
CompressionStrategy <|.. RarCompressionStrategy
CompressionStrategy <|.. SevenZCompressionStrategy
CompressionContext --> CompressionStrategy

```

### Code Java
```java
public interface CompressionStrategy {
    void compresser(String fichier);
}

public class ZipCompressionStrategy implements CompressionStrategy {

    @Override
    public void compresser(String fichier) {
        System.out.println("Compression ZIP du fichier : " + fichier);
    }
}

public class RarCompressionStrategy implements CompressionStrategy {

    @Override
    public void compresser(String fichier) {
        System.out.println("Compression RAR du fichier : " + fichier);
    }
}

public class SevenZCompressionStrategy implements CompressionStrategy {

    @Override
    public void compresser(String fichier) {
        System.out.println("Compression 7z du fichier : " + fichier);
    }
}

public class CompressionContext {

    private CompressionStrategy strategy;

    public void setStrategy(CompressionStrategy strategy) {
        this.strategy = strategy;
    }

    public void compresserFichier(String fichier) {
        if (this.strategy == null) {
            System.out.println("Aucune stratégie sélectionnée.");
            return;
        }

        this.strategy.compresser(fichier);
    }
}

public class Demo {
    public static void main(String[] args) {
        CompressionContext ctx = new CompressionContext();

        ctx.setStrategy(new ZipCompressionStrategy());
        ctx.compresserFichier("rapport.docx");

        ctx.setStrategy(new RarCompressionStrategy());
        ctx.compresserFichier("photo.png");

        ctx.setStrategy(new SevenZCompressionStrategy());
        ctx.compresserFichier("archive.log");
    }
}
```

---

## Liens utiles
- [https://refactoring.guru/design-patterns/strategy](https://refactoring.guru/design-patterns/strategy)
- [https://en.wikipedia.org/wiki/Strategy_pattern](https://en.wikipedia.org/wiki/Strategy_pattern)

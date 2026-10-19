---
layout: default
title: "Principes SOLID"
nav_order: 8
published: true
---

# Principes SOLID

Les principes **SOLID** regroupent des bonnes pratiques de conception orientée objet visant à produire un code plus **compréhensible**, **facile à tester**, **évolutif** et **maintenable**. Ils aident à réduire le couplage inutile, à améliorer la cohésion des classes et à faciliter les modifications ultérieures sans effet domino.

## Historique
Les principes **SOLID** ont été formalisés et popularisés au tournant des années 2000 par Robert C. Martin (a.k.a. « Uncle Bob ») dans son article *Design Principles and Design Patterns*. L’acronyme **SOLID** a été proposé par Michael Feathers pour regrouper cinq idées clés : la **responsabilité unique** (*Single Responsibility Principle*), l’**ouvert/fermé** (*Open/Closed Principle*), la **substitution de Liskov** (*Liskov Substitution Principle*), la **ségrégation des interfaces** (*Interface Segregation Principle*), et l’**inversion des dépendances** (*Dependency Inversion Principle*).

Ces notions ont elles-mêmes été dérivées de concepts existants : Bertrand Meyer avait déjà formulé le principe **ouvert/fermé** en 1988 dans *Object‑Oriented Software Construction* ; Barbara Liskov, puis Jeannette Wing, ont précisé la **substituabilité** par le biais du sous‑typage comportemental (1987–1994). Au début des années 2000, Craig Larman a rapproché l’**OCP** du patron *Protected Variations*, qui promeut la protection des points de variation via des interfaces stables—une idée cohérente avec l’« information hiding » de David Parnas (1972).

Aujourd'hui, les principes **SOLID** sont globalement acceptés comme des principes fondamentaux permettant de concevoir des systèmes extensibles et maintenables qui facilitent l'écriture de tests unitaires.

## Objectifs
- **Maîtriser la complexité croissante** des systèmes : encourager une architecture modulaire et testable.
- **Réduire le couplage et augmenter la cohésion** : isoler les responsabilités pour limiter les effets de bord.
- **Faciliter l’extension sans régression** : permettre d’ajouter des comportements sans modifier le code déjà testé / validé.
- **Préserver les contrats et invariants** : garantir la substituabilité et la stabilité des hiérarchies de types.
- **Améliorer la maintenabilité et la longévité** : rendre le code plus simple à faire évoluer, en particulier lorsqu'il est partagé entre plusieurs équipes.

## Définition
- **SRP - Responsabilité unique** (*Single Responsibility Principle*) : Une classe ne doit avoir qu’une seule raison de changer.
- **OCP - Ouvert/Fermé** (*Open/Closed Principle*) : Les entités ouvertes à l’extension, fermées à la modification.
- **LSP - Substitution de Liskov** (*Liskov Substitution Principle*) : Les types dérivés se substituent aux types de base sans altérer le comportement attendu.
- **ISP - Ségrégation des interfaces** : Plusieurs petites interfaces spécifiques plutôt qu’une interface généraliste.
- **DIP - Inversion des dépendances** (*Dependency Inversion Principle*) : Dépendre d’abstractions plutôt que de classes concrètes.

{: .highlight}
> En pratique, on applique ces principes **ensemble** : SRP pour séparer les rôles, ISP pour cibler les interfaces, DIP pour dépendre de contrats, LSP pour garantir la substituabilité, OCP pour étendre sans toucher au code existant.

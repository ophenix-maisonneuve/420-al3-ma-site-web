---
layout: default
title: "Introduction à UML"
nav_order: 9
has_children: true
published: true
---

# Introduction à UML

Le **Unified Modeling Language** (*UML*) est un langage de modélisation **visuel**, **standardisé** et **orienté objet** permettant de représenter la structure, le comportement et les interactions d’un système logiciel. Il sert de **langue commune** entre les concepteurs, développeurs, analystes métier et architectes. UML est géré par l’**Object Management Group (OMG)** et sa version formelle la plus récente est **UML 2.5.1**, publiée en 2017.

## Historique

Dans les années 1990, plusieurs méthodes de modélisation orientées objet coexistaient (Booch, OMT, Objectory), ce qui fragmentait le domaine. Grady Booch, James Rumbaugh et Ivar Jacobson, surnommés les *Three Amigos*, ont fusionné leurs approches pour créer une méthode unifiée. En **1997**, UML est adopté comme standard par l’OMG. En **2005**, UML devient également une norme ISO/IEC 19501.


## Pourquoi UML ?

UML permet de :
- **Visualiser** le système comme un *blueprint* (plan d’architecture)
- **Communiquer** efficacement avec des équipes non techniques
- **Standardiser** le vocabulaire conceptuel d’un projet
- **Documenter** les décisions d’architecture
- **Analyser** et **planifier** des systèmes complexes

## Types de diagrammes UML

UML regroupe plusieurs types de diagrammes classés en deux familles dont les principaux sont : 

### Diagrammes structurels  
- [Diagramme de classes](../notes/uml-classes)  
- Diagramme de composants
- Diagramme d’objets
- Diagramme de structure composite
- Diagramme de déploiement
- Diagramme de packages
- Diagramme de profils

### Diagrammes comportementaux  
- [Diagramme de séquence](../notes/uml-sequence)  
- Diagramme de cas d’utilisation
- Diagramme d’activité
- Diagramme d’états
- Diagramme de communication
- Diagramme d’interaction globale
- Diagramme de timing.

Dans ce module, nous nous concentrons sur le principal diagramme de chaque famille: **diagramme de classes** (structure) et **diagramme de séquence** (comportemental).

## Outils

Il existe plusieurs outils qui facilitent la création de diagrammes UML, par exemple :

- [draw.io](https://app.diagrams.net/) : Un outil gratuit et très répandu, accessible directement depuis votre navigateur web. Permet de créer facilement différents types de diagrammes UML.

- [Lucidchart](https://www.lucidchart.com/) : Offre une interface intuitive et collaborative en ligne, avec de nombreux modèles prêts à l'emploi.

- [Visual Paradigm](https://www.visual-paradigm.com/) : Une solution complète pour le modélisme UML avec des fonctionnalités avancées telles que la génération de code à partir des diagrammes.

- [StarUML](http://staruml.io/) : Une application multiplateforme spécialisée dans le modélisme UML, idéale pour les projets complexes.

### MermaidJS
[MermaidJS](https://mermaid.js.org/) est une bibliothèque JavaScript open-source qui permet de créer des plusieurs types de diagrammes directement en markdown. MermaidJS supporte les diagrammes UML de séquence, de classe et d'activité. Bien qu'il ne support pas les diagrammes de cas d'utilisation, vous pouvez utiliser le diagramme de flux (flowchart) pour représenter les interactions entre les acteurs et le système.

C'est l'outil choisi pour ces notes de cours, notamment parce qu'il est facile de garder un document markdown sous versionnement avec Git. Cela vous permet de suivre l'évolution de vos diagrammes au fil du temps et de revenir à des versions précédentes si nécessaire.

Vous trouverez plus d'informations sur MermaidJS dans la [documentation officielle](https://mermaid.js.org/)

Pour visualiser les diagrammes MermaidJS dans vos documents Markdown avec VSCode, vous pouvez installer les deux extensions suivantes :

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) : Permet de visualiser les diagrammes MermaidJS dans la prévisualisation Markdown de VSCode.
- [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid) : Permet de visualiser les diagrammes MermaidJS dans la prévisualisation Markdown de VSCode.

### Editeur MermaidJS en ligne
Vous pouvez tester MermaidJS directement dans votre navigateur en utilisant cet [éditeur en ligne](https://mermaid.live/edit).

## Liens utiles
- [https://en.wikipedia.org/wiki/Unified_Modeling_Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language)
- [https://www.omg.org/spec/UML/2.5.1/About-UML/](https://www.omg.org/spec/UML/2.5.1/About-UML/)

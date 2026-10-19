---
layout: default
title: "L'attaque des zombies, la suite"
parent: "Patrons de conception"
nav_order: 8
has_toc: false
published: true
---

# Exercice : L'attaque des zombies, la suite!

Nous sommes en 2036. Il y a maintenant plus de deux ans qu'un mystérieux virus a complètement ravagé la planète et transformé la quasi-totalité de l'humanité en zombies. La grande forteresse de La Prairie, au Québec (Canada), dernier bastion de l'humanité, résiste toujours aux attaques incessantes de hordes de zombies. Les grands sages de la forteresse décident de mettre en place un outil de simulation qui, espèrent-ils, permettra de mieux planifier les attaques et, qui sait, peut-être éventuellement de reprendre le contrôle d'un joyau de l'humanité : le Quartier Dix30 à Brossard. On doit pouvoir lancer la simulation en tant qu'humains (pour prévoir nos défenses) ou en tant que zombies (pour anticiper leurs attaques).


## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/patrons-attaque-zombies-suite.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/patrons-attaque-zombies-suite.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/attaque-zombies-suite-1.0-SNAPSHOT.jar --faction <humains|zombies> [--soldats <nombre de soldats>] [--tireurs <nombre de tireurs>] [--leaders <nombre de leaders>]
```
ou directement à partir de votre IDE.

Il s'agit du code qui simule la situation apocalyptique décrite ci-haut ainsi que les contextes des questions 1 et 2.


## Question 1 : Analyse du code actuel

### 1.1. Différents types de personnages
- Quelles sont les interfaces qui représentent les différents types de personnages ?
- Est-ce qu'il existe toujours un équivalent humain et zombie pour chaque type de personnage ?
   - Si oui, par quelle(s) classe(s) ces équivalents sont-ils représentés ?
   - Sinon, pourquoi ?


### 1.2. Création des personnages
- Comment les différents personnages sont-ils créés dans la version actuelle du code ?
- Cette façon de faire est-elle optimale ?
- Comment pourrait-on s'assurer que l'on crée toujours, de façon simple, les personnages de la bonne faction selon le mode de notre simulation (humain ou zombie) ?


## Question 2 : Mise en place de la `Abstract Factory`

### 2.1. Création de l'interface
- Quelle interface devrez-vous créer pour utiliser le patron `Abstract Factory` ?
- Quelles méthodes devrez-vous ajouter dans l'interface ?
   - ***Indice** : la abstract factory doit permettre de créer chaque type de personnage.*
- Ajoutez cette interface dans votre code

### 2.2. Création des implémentations concrètes
- Quelles implémentations de votre interface devrez-vous créer ?
   - ***Indice** : chaque implémentation est responsable de créer les personnages d'une même faction*
- Créez les implémentations requises dans votre code
- Remplacez les instanciations manuelles dans la classe `Simulation` par l'utilisation de votre Abstract Factory.


### Questions de réflexion
- Est-il maintenant plus simple (ou plus complexe ?) de rouler votre simulation avec une faction différente ? Pourquoi ?
- Si des Cyborgs venus d'une galaxie lointaine venaient prêter main forte aux humains, quelles étapes seraient nécessaires pour les ajouter à votre simulation ? Cela est-il maintenant plus simple grâce à votre Abstract Factory ?

---
layout: default
title: "Structures de données"
nav_order: 9
has_toc: false
published: true
---
# Exercice : Structures de données

![Le Lobby des Braves](../assets/images/lobby-braves.png)

Une ancienne camarade de classe vient de lancer un nouveau jeu de rôle en ligne de type *MMORPG (Massively Multiplayer Online Role-Playing Game)*. N'ayant pas anticipé le succès fulgurant de son nouveau jeu, elle avait concentré tous ses efforts sur un prototype fonctionnel du jeu, mais avait omis toutes les fonctionnalités de lobby (*hub*) qui accompagnent normalement ce type de jeu: gestion des profils de joueurs, salle d'attente, historique des parties, classements, etc.

Connaissant votre expertise en structures de données et en optimisation d’algorithmes, elle vous confie la mission de concevoir ces fonctionnalités. Nom de code : **Le Lobby des Braves**!

## Échéancier

Vous établissez ensemble l'échéancier suivant, échelonné sur quatre semaines (quelle coïncidence!):

- **Semaine 1** : Créer une liste dynamique des joueurs inscrits.
- **Semaine 2** : Mettre en place une file d’attente pour les joueurs et une pile pour l’historique des parties.
- **Semaine 3** : Ajouter un classement des joueurs dans une arborescence pour trier selon le score.
- **Semaine 4** : Permettre l'envoi de messages instantanés par pseudo grâce aux tables de hachage.

Chaque semaine, vous devrez d'abord considérer l'utilisation des structures les plus courantes (sans *thread safety*), puis une alternative qui permettrait la concurrence en cas de besoin. Vous devrez évaluer les forces et les faiblesses de chaque variante des structures afin de déterminer celle qui convient le mieux aux besoins.

## Objectifs

- Explorer et implémenter différentes structures de données : listes, piles, files, arbres, tables.
- Analyser les forces et limites de chaque structure et de leurs implémentations.
- Identifier les cas d’utilisation optimaux pour chaque type.
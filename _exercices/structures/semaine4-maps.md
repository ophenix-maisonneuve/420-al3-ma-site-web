---
layout: default
title: "Structures de données – semaine 4"
parent: "Structures de données"
nav_order: 4
published: true
---

# Exercice : Structures de données – Tables associatives (*Maps*)

## Contexte

Bienvenue, pour la dernière fois, dans **Le Lobby des Braves** ! Votre ultime mission consiste à mettre en place un gestionnaire de messagerie permettant aux joueurs d’échanger des messages instantanés pendant leurs parties. Attention : il ne s’agit pas de créer un serveur de courriels ni une messagerie sophistiquée, mais simplement d’offrir un moyen rapide pour envoyer de brefs messages en temps réel. Dans l’esprit du jeu et pour limiter les coûts d’infrastructure, aucun historique ne sera conservé : une fois le message lu par son destinataire, il disparaît.

Pour vous faciliter la tâche, votre ancienne camarade de classe a amorcé l’implémentation du module. Malheureusement, elle a manqué de temps pour le terminer et, n’ayant pas votre expertise en algorithmes et structures de données, elle doute que ses choix soient toujours optimaux... Comme le temps presse, vous décidez d’abord de compléter son travail pour obtenir une version fonctionnelle, puis d’analyser et optimiser le code afin d’améliorer ses performances.

## Objectifs 

1. **Finaliser/corriger** l’implémentation partielle `GestionnaireMessagerieMap`.
1. **Évaluer** les diverses tables associatives (*maps*) utilisées.
1. **Proposer** des alternatives optimales pour les tables associatives (*maps*).
1. **Analyser** l'implémentation générale du module et optimiser sa performance lorsque requis.

---

## Étapes préparatoires

### 1. Clonez (ou mettez à jour) le dépôt de l’exercice
```bash
git clone git@github.com:ophenix-420-930-ma-24636/lobby-braves.git
```
Ou, si vous avez déjà cloné le dépôt :
```bash
git pull
```

Si vous travaillez sur une autre branche:
```bash
git checkout <votre branche>
git fetch
git merge origin/main
```

En cas de doutes ou de problèmes techniques, la branche `solution` contient tout le code réalisé jusqu'à présent. Elle peut aussi vous servir de point de départ pour cet exercice:
```bash
git checkout solution
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/lobby-braves-1.0-SNAPSHOT.jar
```
Ou :
```bash
mvn clean compile exec:java
```
Ou directement à partir de votre IDE.

### 3. Analysez l’interface `GestionnaireMessagerie` et l'implémentation inachevée `GestionnaireMessagerieMap`
L'interface `GestionnaireMessagerie` définit les méthodes nécessaires au fonctionnement de la messagerie qu'il faudra implémenter :
```java
public interface GestionnaireMessagerie {
    Message envoyer(String emetteur, String destinataire, String contenu, boolean urgent);
    Message prochain(String pseudoDestinataire);
    Collection<Message> purger(String pseudoDestinataire);
    boolean bloquer(String pseudoDestinataire, String pseudoEmetteur);
    boolean debloquer(String pseudoDestinataire, String pseudoEmetteur);
}

```

La classe `GestionnaireMessagerieMap` contient une première ébauche incomplète du gestionnaire de messagerie utilisant une table associative (*map*) :

---

## 1. Terminer l'implémentation de `GestionnaireMesagerieMap`

### 1.1. Implémentez la méthode `purger(destinataire)`
Cette méthode doit supprimer tous les messages adressés au destinataire.
- Quelle méthode de l'interface `Map` doit être utilisée pour cette opération ? 
- Avec l'implémentation actuelle, quelle est la complexité grand O de cette opération ? Pourquoi ?

### 1.2. Implémentez la méthode `bloquer(destinataire, emetteur)`
Cette méthode doit ajouter le pseudo de l'émetteur dans la liste d'utilisateurs bloqués pour le destinataire.
- Quelle méthode de l'interface `Map` doit être utilisée pour retrouver la liste des émetteurs bloqués pour un destinataire ?
- Quelle méthode de l'interface `List` doit être utilisée pour ajouter l'émetteur bloqué à la liste ?
- Avec l'implémentation actuelle, quelle est la complexité grand O de la méthode `bloquer` ?
- Avec l'implémentation actuelle, comment sont gérés les doublons (si par exemple on bloque deux fois le même émetteur pour un même destinataire) ? 
- Est-il pertinent de conserver l'ordre des émetteurs bloqués ? Pourquoi ?

### 1.3. Terminez l'implémentation de la méthode `envoyer`emetteur, destinataire, contenu, urgent)`
Avant d'envoyer un message, il faut maintenant vérifier que l'émetteur n'a pas été bloqué par le destinataire.
- Avec l'implémentation actuelle, quelle est la complexité grand O de la méthode `envoyer` ? Pourquoi ?

### 1.4. Corrigez la méthode `prochain(destinataire)`
Cette méthode doit lire le prochain message du destinataire puis le supprimer. Les messages doivent être triés en ordre chronologique (du plus ancien au plus récent), mais en priorisant toujours les messages urgents.
- Corrigez la méthode pour que l'ordre de tri soit respecté. Quel était le problème ?
- Quel type de liste récupérez-vous lorsque vous allez chercher les messages dans la *map* (en utilisant le destinataire comme clé) ? 
- Quelle est la complexité des opérations suivantes dans la méthode `prochain` :
  - Récupération de la liste des messages dans la *map* ?
  - Tri de la liste ?
  - Retrait du premier message ?
- Quelle est donc la complexité globale de la méthode ?

---

## 2. Choix du type de `Map`
Votre camarade ne semble pas convaincue qu'elle a choisi le bon type de table associative (*map*) pour sa première implémentation. Vous décidez donc de vérifier si un autre type aurait pu être plus performant.

### 2.1 Choix de la `Map` pour le champ `messages`
Répondez aux questions suivantes afin de guider votre évaluation:
- Quelles sont les opérations les plus fréquentes sur la *map* messages ?
- Avec une `TreeMap`, quelle est la complexité grand O de ces opérations dominantes ?
- À quoi est due cette complexité ?
- Avons-nous besoin de la fonctionnalité qui introduit cette complexité supplémentaire ? 
- Existe-t-il un différent type de `Map` offrant une complexité plus faible pour les opérations dominantes ?
  - Si oui, quelle est la complexité des opérations dominantes sur ce type de `Map` ?
  - Sinon, pourquoi ?

**Si vous croyez avoir trouvé une structure plus optimale, effectuez cette modification**

### 2.2 Choix de la `Map` pour le champ `blocages`
Est-ce que votre raisonnement de la question précédente s'applique aussi pour le champ `blocages` ?
- Si oui, pourquoi ?
- Sinon, la structure actuelle est-elle optimale ou en existe-t-il une meilleure ?

**Si vous croyez avoir trouvé une structure plus optimale, effectuez cette modification**

---

## 3. Optimisation générale du module de messagerie
Une fois que vous aurez validé que l'implémentation est complète, utilisez la [méthode vue dans le cours](../notes/optimisation) pour optimiser différents aspects du module de messagerie.

{: .highlight}
> Comme c'est souvent le cas, les étapes 2 (structure) et 3 (algorithme) de la méthode devraient être celles qui permettent les gains le plus significatifs. 

Pour chaque amélioration proposée, expliquez :
- Quelle était la complexité grand O avant et après votre changement
- À quoi est dû le gain de performance
- Dans quelles circonstances votre changement apporterait les plus grands bénéfices

{: .astuce}
> Portez une attention particulière aux différentes structures de données utilisées, ainsi qu'aux algorithmes utilisés pour effectuer des opérations sur celles-ci (ajout, retrait, itération, recherche, etc.).


---

## 4. Concurrence (*thread safety*)
Comme il est fort probable que plusieurs joueurs envoient et lisent des messages simultanément, il serait prudent de garantir la cohérence de notre `GestionnaireMessagerieMap` en contexte concurrent (*multi-thread*).

### 4.1 Identifiez 3 différentes façons de garantir le *thread-safety*
Pour chacune des façons identifiées, quels sont...
- Les situations où cette forme de synchronisation est optimale
- Les situations où cette forme de synchronisation est à éviter

### 4.2 Choix de la méthode de synchronisation
Sachant que le nombre de joueurs inscrits au jeu ne cesse de croître et que l'on s'attent à un grand volume de messages écrits et lus, choisissez l'une des trois méthodes de synchronisation énumérées précédemment.
 - Implémentez votre changement directement dans la classe `GestionnaireMessagerieMap`

{: .astuce}
> Il se peut aussi que votre choix soit une combinaison de plusieurs techniques...
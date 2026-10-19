---
layout: default
title: "Structures de données - semaine 2"
parent: "Structures de données"
nav_order: 2
published: true
---

# Exercice : Structures de données – Piles et Files

## Contexte
Bienvenue à nouveau dans **Le Lobby des Braves** ! Cette semaine, vous devez enrichir votre application en ajoutant des structures pour gérer l’attente des joueurs et l’historique des parties.

En tant qu’administrateur, vous devez pouvoir :
- Maintenir une **pile (LIFO)** pour l’historique des parties existantes.
- Gérer une **file (FIFO)** pour les joueurs en attente.
- Ajouter une **file avec priorité** pour traiter certains joueurs avant les autres.
- Implémenter une version **thread-safe** pour la pile ou la file afin de gérer la concurrence.

## Objectifs
- Implémenter la gestion des piles et files avec différentes structures (`ArrayDeque`, `PriorityQueue`, `ConcurrentLinkedDeque`).
- Analyser leurs forces et faiblesses.
- Comprendre les cas d’utilisation optimaux.

---

## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/lobby-braves.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/lobby-braves.git
```

### 2. Mettez votre dépôt local à jour
Si vous travaillez directement sur la branche main:
```bash
git pull
```

Si vous travaillez directement sur une autre branche:
```bash
git checkout <votre branche>
git fetch
git merge main
```

### 3. Inspectez les trois nouvelles interfaces
- `Provider<T>`pour une classe fournissant des instances de quelque chose (nous l'utiliserons pour `Joueur`)
- `GestionnaireParties` pour la pile des parties.
- `GestionnaireAttente` pour les files.

### 4. Implémentez la méthode `list()` dans chacune de vos implémentations de `GestionnaireJoueurs`
Ceci est nécessaire afin de respecter la nouvelle interface étendue par `GestionnaireJoueurs`.

```java
public interface GestionnaireJoueurs extends Provider<Joueur>
```

La méthode `list()` doit retourner la liste des joueurs disponibles.
- Trouvez une façon de retourner une version **immuable** de la liste de joueurs.

{: .astuce}
> La classe utilitaire `Collections` a sûrement quelque chose d'utile pour cela...

{: .highlight}
> Il aurait été possible d'ajouter directement la méthode `list()` à l'interface `GestionnaireJoueurs`. Cependant, en l'ajoutant à une interface différente, on sépare les responsabilités. Ainsi, si on désire permettre qu'une classe récupère la liste des joueurs, sans toutefois lui permettre de faire des opérations de gestion, on pourrait lui passer un paramètre de type `Provider<Joueur>` au lieu de lui donner accès à toutes les méthodes de l'interface `GestionnaireJoueurs`.


---

## 1. Pile LIFO

Certains cas de tricherie ont été détectés dans le jeu! Des petits malins ont exploité un bogue leur permettant d'attaquer leurs adversaires pendant qu'ils magasinent auprès d'un marchand d'armes ou d'armures et qu'ils sont complètement sans défense. La conceptrice du jeu (votre ancienne camarade de classe) vous confie donc le mandat de permettre la suppression de certaines parties afin que leur score ne soit plus comptabilisé. Mais attention: il est malheureusement impossible de supprimer une partie sans risquer la corruption des parties s'étant déroulées ensuite. Il vous faudra donc utiliser une pile pour conserver l'historique des parties. En cas de nécessité, la pile pourra être **dépilée** de façon à supprimer toutes les parties ayant débuté après celle où la tricherie a eu lieu.

### 1.1. Créez une première implémentation de `GestionnaireParties` nommée `GestionnairePartiesArrayDeque`
Cette implémentation doit utiliser une `ArrayDeque` pour gérer la pile.
- Gardez une référence vers une nouvelle `ArrayDeque<Partie>` comme champ dans la classe.
- Aurait-il été possible d'utiliser une `LinkedList` à la place d'une `ArrayDeque` pour gérer la pile ? 
    - ***Indice**: observez les interfaces implémentées par `LinkedList`...
- Pourquoi `ArrayDeque` est-elle plus efficace qu’une `LinkedList` pour une pile ?

### 1.2. Implémentez la méthode `ajouter(Partie partie)` dans `GestionnairePartiesArrayDeque`
- Quelle méthode utilisez-vous pour **empiler** une partie sur la pile ?
- Quelle est la complexité grand O de cette méthode ?
- Quelle est la différence entre les méthodes `push` et `addFirst` de l'interface `Deque` (et de son implémentation `ArrayDeque`) ?
- Dans une utilisation d'une `Deque` en tant que pile, est-il préférable d'utiliser l'une ou l'autre de ces méthodes ? Pourquoi ?

### 1.3. Vérifiez la date de création avant l'ajout
Avant d'ajouter une partie sur la pile, assurez-vous maintenant que la partie précédente (celle qui est sur le dessus de la pile) a une date de création plus vieille que la partie à empiler. En d'autres mots, vous voulez ajouter une sécurité empêchant l'ajout d'une vieille partie (possiblement obsolète) sur le dessus de la pile. 
- Quelle méthode utilisez-vous pour récupérer l'élément du dessus de la pile **sans le supprimer** ?
- Quelle est la complexité grand O de la méthode utilisée ? 

### 1.4. Implémentez la méthode `inspecterHistorique(Partie partie)` dans `GestionnairePartiesArrayDeque`
Cette méthode doit retourner toutes les parties ayant débuté après la partie passée en paramètre. En cas de suppression, ces parties seraient donc toutes supprimées.
- Pouvez-vous utiliser la même méthode que celle utilisée à la question précédente (1.3) pour établir l'historique ? Pourquoi ?
- Implémentez la récupération de l'historique
  - ***Indice**: observez les interfaces implémentées par `ArrayDeque`
- Quelle est la complexité grand O de la méthode que vous avez implémentée ?

### 1.5. Implémentez la méthode `supprimerPartie(Partie partie)` dans `GestionnairePartiesArrayDeque`
Cette méthode doit supprimer la partie passée en paramètre ainsi que toutes les parties plus récentes (c'est-à-dire celles par-dessus dans la pile).
- Quelle méthode utilisez-vous pour récupérer et supprimer (**dépiler**) l'élément du dessus de la pile ?
- Quelle est la complexité grand O de cette méthode ?
- Quelle est la différence entre les méthods `pop` et `removeFirst` de l'interface `Deque` (et de son implémentation `ArrayDeque`) ?
- Selon vous, dans un scénario d'utilisation en tant que pile, laquelle de ces méthodes est préférable ? Pourquoi ?

---

## 2. Pile LIFO avec concurrence (*thread-safety*)

### 2.1. Créez une nouvelle implémentation de `GestionnaireParties` nommée `GestionnairePartiesConcurrent`
Cette implémentation doit utiliser une `ConcurrentLinkedDeque` pour gérer la pile.
- Gardez une référence vers une nouvelle `ConcurrentLinkedDeque<Partie>` comme champ dans la classe.

### 2.2. Implémentez les méthodes `ajouter(Partie partie)`, `inspecterHistorique(Partie partie)` et `supprimerPartie(Partie partie)` dans `GestionnairePartiesConcurrent`
L'implémentation sera identique à celle de `GestionnairePartiesArrayDeque`

### 2.3. Explorez les alternatives
- Si on suppose un risque que deux administrateurs lancent une suppression simultanément, est-ce que l'utilisation de `ConcurrentLinkedDeque` offre une protection suffisante ? Pourquoi ?
- Existe-t-il une méthode utilitaire dans `Collections` qui peut aider ?
  - Si oui, laquelle ?
  - Sinon, pourquoi les méthodes existantes ne le permettent pas ?
- À quoi sert le mot-clé `synchronized` en Java ? Comment pourriez-vous l'utiliser pour ce scénario ?

### 2.4 Ajoutez la synchronisation nécessaire dans la classe `GestionnairePartiesConcurrent`
Vous devez vous assurer que si une suppression est déjà en cours, un 2e administrateur ne peut pas supprimer de la pile tant que la premire suppression n'est pas terminée.
- Pourquoi ce mécanisme additionnnel est-il requis ici ?
- Pourquoi `ConcurrentLinkedDeque` ne suffit pas pour garantir l’intégrité lors d’une suppression en cascade ?

---

## 3. File d'attente (FIFO)
Le jeu de votre collègue est victime de son succès! Malgré plusieurs optimisations et l'ajout de ressources matérielles, l'infrastructure actuelle a de la difficulté à supporter le nombre croissant de joueurs. En attendant d'obtenir le financement pour l'achat de serveurs encore plus puissants, vous décidez de mettre en place une file d'attente où, lors des périodes les plus achalandées, les joueurs devront attendre leur tour avant de participer à une partie.

### 3.1. Créez une nouvelle implémentation de `GestionnaireAttente` nommée `GestionnaireAttenteDeque`
Cette implémentation doit d'abord utiliser une `LinkedList` pour gérer la file d'attente.
- Déclarez un champ de type `Deque<Joueur>` dans la classe et instanciez-le en tant que `LinkedList<Joueur>`.
```java
private final Deque<Joueur> file = new LinkedList<>();
```
- Quel est l'avantage de procéder ainsi plutôt que de déclarer le champ comme `List` ou `ArrayList` ?

### 3.2 Implémentez la méthode `ajouter(Joueur joueur)` dans `GestionnaireAttenteDeque`
Cette méthode doit ajouter un joueur dans la file d'attente selon les règles d'une file (donc à la fin).
- Quelle méthode de l'interface `Deque` doit-on utiliser pour **mettre en file** (*enqueue*) un joueur ?
   - ***Indice***: consultez le tableau sur l'utilisation de la `Deque` en tant que file d'attente dans les [notes de cours](../notes/deque)
- Quelle est la complexité grand O de votre implémentation avec la structure utilisée (`LinkedList`) ?
- Quelle est la différence entre les méthodes `add` et `addLast` de l'interface `Deque` ?
- Pourquoi l'instanciation suivante fonctionne aussi ?
```java
private final Queue<Joueur> file = new LinkedList<>();
```
- En lien avec vos réponses ci-haut, y aurait-il un avantage à utiliser l'interface `Queue` plutôt que l'interface `Deque` pour un usage strictement en tant que file ?
  - Si oui, effectuez la modification dans votre code.

### 3.3 Implémentez la méthode `prochain()` dans `GestionnaireAttenteDeque`
Cette méthode doit **sortir de la file** (*dequeue*) le prochain joueur afin de lui permettre de joindre une partie.
- Quelle méthode de l'interface `Queue` doit-on utiliser pour **sortir de la file** (*dequeue*) un joueur ?
  - ***Indice***: consultez le tableau sur l'utilisation de la `Deque` en tant que file d'attente dans les [notes de cours](../notes/deque)
- Quelle est la complexité grand O de cette méthode avec la structure utilisée (`LinkedList`) ?


### 3.4 Ajoutez la gestion de l'inactivité à la méthode `prochain()` dans `GestionnaireAttenteDeque`
Si le prochain joueur à **sortir de la file** (*dequeue*) est inactif depuis plus de 5 minutes, on doit le retourner à la fin de la file pour éviter de retarder davantage le début d'une prochaine partie. On doit procéder ainsi jusqu'à ce que l'on trouve le premier joueur qui n'est pas inactif depuis plus de 5 minutes. Vous pouvez utiliser le getter `Joueur.getDerniereActivite()` pour obtenir le moment de la dernière activité du joueur. Pour les fins de l'exercice, ce champ a une valeur aléatoire comprise entre le moment de sa création et 10 minutes plus tôt.
- Quelle méthode de l'interface `Queue` doit-on utiliser pour **inspecter** le prochain joueur ?
  - ***Indice***: consultez le tableau sur l'utilisation de la `Deque` en tant que file d'attente dans les [notes de cours](../notes/deque)
- Est-ce que ce nouveau critère a modifié la complexité grand O (pire cas) de la méthode `prochain()` ? Pourquoi ?

### 3.5. Implémentez la méthode `afficher` dans `GestionnaireAttenteDeque`
Cette méthode doit itérer sur la file et afficher chacun des joueurs dans l'ordre. Vous pouvez utiliser un `Iterator` ou une boucle `for-each`.
- Quelle est la complexité grand O de votre implémentation ?
- Si, au lieu d'afficher la liste entière, on avait voulu afficher seulement le premier élément de la file, quelle méthode de l'interface `Queue` aurait-on pu utiliser ?
- Dans ce cas, quelle aurait été la complexité grand O de la méthode `afficher` ?

### 3.6. Remplacez la `LinkedList` par une `ArrayDeque`
Après l'implémentation, vous réalisez qu'une `ArrayDeque` aurait possiblement été légèrement plus performante qu'une `LinkedList` pour l'implémentation de `GestionnaireAttenteDeque`
- Remplacez le champ `file` pour qu'ils utilise plutôt une `ArrayDeque`.
- Quelle(s) modification(s) devez-vous faire dans le code pour que tout fonctionne ? Pourquoi ?

---

##  4. File d'attente prioritaire
La file d'attente que vous avez mise en place donne de bons résultats. Cependant, certains joueurs parmi les plus anciens se disent frustrés de la situation, car il leur arrive d'attendre plus longtemps avant de pouvoir démarrer une partie. Afin de les remercier d'avoir largement contribué au succès fulgurant du jeu, vous décidez de modifier la file afin d'accorder la priorité par ancienneté plutôt qu'au premier arrivé (*first in first out*).

### 4.1. Créez une nouvelle implémentation de `GestionnaireAttente` nommée `GestionnaireAttentePrioritaire`
Cette implémentation doit utiliser une `PriorityQueue` pour gérer la pile.
- Déclarez un champ de type `Queue<Joueur>` dans la classe et instanciez-le en tant que `PriorityQueue<Joueur>`.
```java
private final Queue<Joueur> file = new PriorityQueue<>();
```
- À votre avis, pourquoi `PriorityQueue` ne peut pas implémenter l'interface `Deque` ?
- En ce moment, comment sera triée la file ? En d'autres mots, comment sera établie la priorité ?

### 4.2. Ajoutez un `Comparator` permettant d'accorder la priorité par ancienneté
Ce `Comparator` doit comparer le champ `inscription` de deux joueurs afin de déterminer le plus ancien et accorder la priorité à ce dernier.
- Passez votre comparator au constructeur de `PriorityQueue`
```java
private final Queue<Joueur> file = new PriorityQueue<>(votreComparateur);
```

### 4.3. Implémentez la méthode `ajouter(Joueur joueur)` dans `GestionnaireAttentePrioritaire`
Cette méthode doit ajouter un joueur dans la file d'attente selon les règles d'une file (donc à la fin).
- La **mise en file** (*enqueue*) d'un joueur est-elle O(1) avec une file prioritaire ? Pourquoi ? 

### 4.4. Implémentez la méthode `prochain()` dans `GestionnaireAttentePrioritaire`
Cette méthode doit **sortir de la file** (*dequeue*) le prochain joueur afin de lui permettre de joindre une partie.
- Le **retrait de la file** (*dequeue*) d'un joueur est-il O(1) avec une file prioritaire ? Pourquoi ?

### 4.5. Implémentez la méthode `afficher()` dans `GestionnaireAttentePrioritaire`
Cette méthode doit itérer sur la file et afficher chacun des joueurs dans l'ordre. Vous pouvez utiliser un `Iterator` ou une boucle `for-each`.
- Quelle est la complexité grand O de cette méthode ?

### 4.6. Observez la différence entre l'ordre dans la file et l'ordre de retrait
- À l'aide du menu principal :
  - Ajoutez quelques joueurs (idéalement une dizaine pour s'assurer que l'exemple soit éloquent)
  - Faites afficher la file d'attente
  - Retirez les joueurs de la file d'attente un par un
- Comparez l'ordre d'affichage des joueurs avec l'ordre dans lequel ils ont été retirés. Qu'observez-vous ?
- Pourquoi observez-vous ce comportement ?

---

## 5. Bonus: File d'attente avec concurrence

### 5.1. File FIFO *thread-safe*
- Selon vous, existe-t-il une structure que nous avons déjà étudiée qui serait aussi convenable pour une file d'attente FIFO en contexte *multi-thread*
  - Si oui, laquelle et pourquoi ?
  - Si non, pourquoi ?


### 5.2. File prioritaire *thread-safe*
Analysez la documentation de la classe `PriorityBlockingQueue`. 
- Quelle est la différence entre une file bloquante et une file non-bloquante ?
- Cette structure pourrait-elle être utilisée à la place d'une `PriorityQueue` dans un contexte *multi-thread* ?
- Quel est la performance de cette structure pour les opérations de **mise en file** et de **retrait de la file** ?

### 5.3. Créez une nouvelle implémentation de `GestionnaireAttente` nommée `GestionnaireAttentePrioritaireConcurrent`
Cette implémentation doit utiliser une `PriorityBlockingQueue` pour gérer la pile.
- Déclarez un champ de type `BlockingQueue<Joueur>` dans la classe et instanciez-le en tant que `PriorityBlockingQueue<Joueur>`.
```java
private final BlockingQueue<Joueur> file = new PriorityBlockingQueue<>();
```
- Remarquez les nouvelles méthodes suivantes qui sont maintenant disponibles: `take()` et `put(Joueur j)`
- Quelle est la différence entre ces méthodes et les méthodes `remove()` et `add(Joueur j)` d'une file standard ?

### 5.4. Implémentez les méthodes `ajouter(Joueur joueur)`, `prochain()` et `afficher()` de `GestionnaireAttentePrioritaireConcurrent`
- L'implémentation de `afficher()` sera identique aux implémentations précédentes.
- Comment pouvez-vous vous assurer qu'un appel à `prochain()` attendra qu'un élément soit disponible avant de retourner cet élément ?
- Comment pouvez-vous vous assurer qu'un appel à `ajouter(Joueur j)` bloquera jusqu'à ce que la file soit accessible pour l'insertion ?



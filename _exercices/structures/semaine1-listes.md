---
layout: default
title: "Structures de données - semaine 1"
parent: "Structures de données"
nav_order: 1
published: true
---

# Exercice : Structures de données - Listes

## Contexte
Bienvenue au **Lobby des Braves**! Vous devez créer une application qui gère les joueurs inscrits au jeu de rôle en ligne. Cette semaine, nous allons nous concentrer sur la gestion de la liste des joueurs inscrits.

## Objectifs
Implémenter la gestion des joueurs dans plusieurs types de listes différentes afin de déterminer les forces et les faiblesses de chaque implémentation.


## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/lobby-braves.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/lobby-braves.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/lobby-braves-1.0-SNAPSHOT.jar
```
ou
```bash
mvn clean compile exec:java
```
ou directement à partir de votre IDE.

Familiarisez-vous avec le menu, qui vous affichera les opérations que nous devrons être en mesure d'exécuter lorsque l'application sera complétée. Pour l'instant, la plupart des options lanceront une `UnsupportedOperationException` ou ne feront simplement rien. Au fur et à mesure du développement, les options commenceront à fonctionner.

### 3. Analysez l'interface `GestionnaireJoueurs` et son implémentation fournie `GestionnaireJoueursListeChainee`
- L'implémentation `GestionnaireJoueursListeChainee` fait-elle usage des Collections fournies par Java ?
   - Si oui, lesquelles ?
   - Si non, comment gère-t-elle la liste ?

{: .highlight}
> Certaines opérations ne sont pas implémentées de manière optimale dans cette version.

## 1. LinkedList

### 1.1. Créez une nouvelle implémentation de l'interface `GestionnaireJoueurs` appelée `GestionnaireJoueursLinkedList`
Cette implémentation doit utiliser une `LinkedList` pour gérer la liste au lieu d'une gestion manuelle.
- Gardez une référence vers une nouvelle `LinkedList` comme champ dans la classe.
- Toutes les opérations des questions suivantes manipuleront cette liste.
- Changez l'initialisation du champ `Lobby.gestionnaireJoueurs` pour qu'il utilise votre nouvelle implémentation.
```java
private GestionnaireJoueurs gestionnaireJoueurs = new GestionnaireJoueursLinkedList();
```

### 1.2. Implémentez la méthode `ajouter(Joueur)` dans `GestionnaireJoueursLinkedList`
Le joueur doit être ajouté à la fin de la liste.
- Quel est le comportement par défaut de la méthode `add()` sur une `List` ? 
- Si vous aviez voulu insérer ailleurs qu'à la fin, comment auriez-vous pu faire ?

### 1.3. Implémentez la méthode `supprimer(Joueur)` dans `GestionnaireJoueursLinkedList`
Cette méthode doit supprimer un `Joueur` s'il possède le même pseudo que l'instance de `Joueur` passé en paramètre.
- Est-ce qu'un appel à `List.remove(joueur)` fonctionne ? Pourquoi ?
- Modifiez le code de la classe `Joueur` pour que l'appel à `List.remove(joueur)` fonctionne correctement
  - ***Indice:** il s'agit d'une méthode à ré-implémenter dans la classe `Joueur`*


### 1.4. Implémentez la méthode `afficher()` dans `GestionnaireJoueursLinkedList`
Cette méthode doit parcourir la liste dans son ordre actuel (sans faire de tri) et afficher le joueur en utilisant `System.out.println(joueur)`
- Utilisez un `Iterator` (ou une boucle `for-each`) pour itérer sur la liste et afficher les informations de chaque joueur.
- Est-ce que les informations sur les joueurs apparaissent dans un format lisible ?
- Modifiez le code de la classe `Joueur` pour que l'information affichée soit lisible et utilisable.
  - ***Indice:** il s'agit d'une méthode à ré-implémenter dans la classe `Joueur`*
- Dans quel ordre les joueurs sont-ils affichés ?


### 1.5. Implémentez la méthode `trier(Ordre)` dans `GestionnaireJoueursLinkedList`
- Quel algorithme de tri est utilisé lors de l'appel à `List.sort()` ?
- Est-il possible de forcer un algorithme différent ? Pourquoi ?
- Comment la méthode `List.sort()` est-elle capable de déterminer sur quel(s) champ(s) de la classe `Joueur` se baser pour établir l'ordre?
- Existe-t-il une façon simple de trier la liste en ordre décroissant plutôt qu'en ordre croissant ?
  - ***Indice:** il suffit d'appeler une méthode utilitaire `Collections.<quelque chose>` après avoir fait le tri.*
- Utiliser cette façon de faire pour implémenter le tri en ordre inverse lorsque le paramètre `Ordre.INVERSE` est passé à la méthode `trier`


## 2. ArrayList

### 2.1. Créez une nouvelle implémentation de l'interface `GestionnaireJoueurs` appelée `GestionnaireJoueursArrayList`
Cette implémentation doit utiliser une `ArrayList` pour gérer la liste au lieu d'une gestion manuelle.
- Gardez une référence vers une nouvelle `ArrayList` comme champ dans la classe.
- Toutes les opérations des questions suivantes manipuleront cette liste.

### 2.2. Implémentez la méthode `ajouter(Joueur)` dans `GestionnaireJoueursArrayList`
Le joueur doit être ajouté à la fin de la liste.
- Comment la performance de l'ajout en fin de liste se compare-t-elle à celle de `GestionnaireJoueursLinkedList` ?
- Qu'en est-il de l'ajout en début ou en milieu de liste ?

### 2.3. Implémentez la méthode `trier(Joueur)` dans `GestionnaireJoueursArrayList`
Le tri peut simplement être délégué à la méthode `List.sort()`.
- Comment la performance théorique du tri se compare-t-elle à celle de `GestionnaireJoueursLinkedList` ? Pourquoi ?

### 2.4. Implémentez la méthode `supprimer(Joueur)` dans `GestionnaireJoueursArrayList`
Cette méthode doit supprimer un `Joueur` s'il possède le même pseudo que l'instance de `Joueur` passé en paramètre. Plutôt que de simplement appeler `List.remove(joueur)`, essayez les étapes suivantes:
- Utilisez `Collections.binarySearch(liste, joueur)` pour récupérer l'index de l'élément à supprimer.
- Supprimez l'élément trouvé à l'aide de `List.remove(int index)`
- Est-ce que cela fonctionne directement ?
  - Si oui, pourquoi ?
  - Sinon, effectuez le changement qui permettra à la recherche de fonctionner.
- Aurait-il été efficace d'implémenter la suppression de cette manière avec `GestionnaireJoueursLinkedList` ? Pourquoi ?

### 2.5. Implémentez la méthode `afficher()` dans `GestionnaireJoueursArrayList`
Cette méthode doit parcourir la liste dans son ordre actuel (sans faire de tri) et afficher le joueur en utilisant `System.out.println(joueur)`
- Utilisez une boucle `for` avec un index (`for (int i = 0; i < liste.size(); i++)`) pour itérer sur la liste et afficher les informations de chaque joueur.
- La boucle ci-haut aurait-elle été plus performante, moins performante, ou de performance égale avec `GestionnaireJoueursLinkedList` ? Pourquoi ?

## 3. Concurrence (*thread-safety*)

### 3.1. Créez une nouvelle implémentation de l'interface `GestionnaireJoueurs` appelée `GestionnaireJoueursSynchronise`
Cette implémentation doit utiliser une implémentation de `List` qui est *thread-safe* pour gérer la liste de joueurs.
- Si on suppose que les profils des joueurs seront consultés très fréquemment, mais que les ajouts et suppressions seront relativement rares, quelle option parmi les suivantes serait préférable ? Pourquoi ?
   - `Collections.synchronizedList(List)`
   - `CopyOnWriteArrayList`

### 3.2. Implémentez les méthodes `ajouter(Joueur)`, `supprimer(Joueur)`, `afficher()` et `trier(Joueur)`
Les méthodes peuvent simplement déléguer l'opération à l'implémentation de liste sous-jacente que vous aurez choisie.
- Avez-vous quelque chose de particulier à faire pour vous assurer que votre implémentation de `GestionnaireJoueursSynchronise` se comportera correctement en situation concurrente (*multi-thread*) ?

## 4. Bonus

### 4.1. Modifiez le critère de comparaison pour la méthode `trier(Joueur)`
Pour l'une de vos implémentations (`GestionnaireJoueursLinkedList`, `GestionnaireJoueursArrayList`, `GestionnaireJoueursSynchronise`), utilisez la méthode `List.sort(Comparator<T>)` pour trier la liste selon le **score** du joueur plutôt que son **pseudo**.

{: .astuce}
> L'interface [Comparator<T>](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Comparator.html) peut être utilisée pour modifier la façon dont les objets sont comparés, et donc triés, dans une liste. 

Exemple en Java "classique" :
```java

// Comparator comme objet (classe anonyme)
Comparator<String> comparateur = new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
};

Collections.sort(liste, comparateur);
```

Exemple en Java moderne (avec expressions **lambda**)
```java
Collections.sort(liste, (s1, s2) -> Integer.compare(s1.length(), s2.length()));
```


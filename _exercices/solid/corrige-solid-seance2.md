---
layout: default
title: "SOLID - Corrigé séance 2"
parent: "Principes SOLID"
nav_order: 2
has_toc: false
published: true
---

## 3. LSP - Principe de substitution de Liskov

### 3.1. Que se passera-t-il si l'entrée (`input`) est `null` dans les cas suivants :
**Si `puzzle` est de type `PuzzleBase` ?**
Si `puzzle` est de type `PuzzleBase`, tout fonctionnera comme prévu : comme `input == null`, la méthode `attempt` retournera `false`, qui se propagera à l'appelant de `PuzzleRunner.run`.

**Si `puzzle` est de type `LaserMazePuzzle` ?**
Si `puzzle` est de type `LaserMazePuddle`, on est dans le trouble : comme `input == null`, la méthode `attempt` lancera une exception qui n'est pas correctement gérée par l'appelant. Le résultat très probable sera un crash de l'application.

### 3.2. En fonction de vos réponses ci-haut, le principe de substitution de Liskov est-il respecté ? Pourquoi ?
Le principe LSP n'est donc pas respecté, puisque le comportement change drastiquement (au point de briser l'application) lorsqu'une classe de base est remplacée par l'un de ses enfants.

### 3.3. Si le principe n'est pas respecté, proposez une nouvelle version du code qui corrige le problème.

```java

// Contrat de base stable
public class PuzzleBase {
    public boolean attempt(String input) {
        if (input == null) {
            return false;
        }
        return input.length() >= 1;
    }
}

// LaserMaze permet les entrées START et RUN, mais cette fois-ci sans lancer des exceptions
public class LaserMazePuzzle extends PuzzleBase {
    @Override
    public boolean attempt(String input) {
        if (!("START".equals(input) || "RUN".equals(input))) {
            return false;
        }

        // exécuter de la logique métier ici au besoin
        return true;
    }
}

// Code client conscient des capacités
public class PuzzleRunner {
    public boolean run(PuzzleBase puzzle, String input) {
        return puzzle.attempt(input);
    }
}
```

## 4. ISP - Principe de ségrégation des interfaces

### 4.1. Pourquoi le code ci-haut enfreint le principe de ségrégation des interfaces ?
L'interface `GameConsole` tente d'en faire trop, et force les implémentations à implémenter des méthodes inutilement pour des fonctionnalités qui ne sont pas prises en charge. La quantité d'exceptions `UnsupportedOperationException` est un très bon indice que nous sommes en présence de code qui enfreint le principe de ségrégation des interfaces : il serait préférable d'avoir des interfaces plus petites pouvant être implémentées "à la pièce".

### 4.2. Proposez une nouvelle version du code qui corrige le problème.
La clé de la solution ci-bas consiste au découpage de la grosse interface en petites interfaces ciblées. Très souvent, le principe ISP ressemble beaucoup au principe SRP, mais appliqué spécifiquement aux interfaces (lorsque le langage utilisé permet les interfaces).

```java
public interface TextOutput {
    void displayText(String text);
}

public interface AudioOutput {
    void playSound(String soundFile);
}

public interface HapticFeedback {
    void vibrate(int intensity);
}

public interface MapDisplay {
    void showMap(String roomId);
}

public interface VideoRecorder {
    void recordVideo(String filePath);
}

public class TextTerminal implements TextOutput {
    public void displayText(String text) {
        System.out.println(text);
    }
}

public class TelephoneIntelligent implements TextOutput, AudioOutput, HapticFeedback, MapDisplay, VideoRecorder {
    public void displayText(String text) {
        System.out.println(text)
    }

    public void playSound(String soundFile) {
        // implémentation "dummy", la vraie implémentation devrait jouer le son
        System.out.println("Play sound: " + soundFile);
    }

    public void vibrate(int intensity) {
        // implémentation "dummy", la vraie implémentation devrait faire vibrer l'appareil
        System.out.println("Vibrate: " + intensity);
    }

    public void showMap(String roomId) {
        // implémentation "dummy", la vraie implémentation devrait afficher une carte
        System.out.println("Map room: " + roomId);
    }

    public void recordVideo(String filePath) {
        // implémentation "dummy", la vraie implémentation devrait enregistrer la vidéo
        System.out.println("Record to: " + filePath);
    }
}
```

## 5. DIP - Principe d'inversion des dépendances

### 5.1. Supposez que l'on désire maintenant proposer la sauvegarde dans une base de données plutôt que dans un fichier. 

**Avec le code actuel, quelle(s) modification(s) serait(ent) nécessaire(s) pour prendre en charge ce nouveau type de sauvegarde ?**
1. Créer une nouvelle classe `DatabaseProgressRepository`
2. Modification de la classe `ProgressService` comme ceci:

```java
private DatabaseProgressRepository repository;
public ProgressService() {
    this.repository = new DatabaseProgressRepository();
}
```

3. Se croiser les doigts que `DatabaseProgressRepository` implémente exactement la méthode `saveProgress(String, int, int)`


**Cette modification enfreindrait-elle l'un des 4 autres principes (S-O-L-I) ? Pourquoi ?**

Cette modification enfreindrait le principe OCP (ouvert/fermé), car on doit modifier directement `ProgressService` pour ajouter une fonctionnalité.


**Avec le code actuel, serait-il possible de prendre en charge à la fois la sauvegarde dans un fichier et la sauvegarde en BD sans changer le code de `ProgressService` ? Pourquoi ?**

Non, pas sans des modifications très significatives dans la classe `ProgressService`, incluant possiblement un changement de la signature de méthode `save` pour passer en paramètre le type de sauvegarde (fichier, BD).

### 5.2. En fonction de vos réponses aux questions précédentes, croyez-vous que le code ci-haut respecte le principe d'inversion des dépendances ?
Non. Le code de `ProgressService`, qui définit la logique métier, dépend directement d'une implémentation technique et y est donc fortement couplé. 

### 5.3. Si le code ne respecte pas DIP, proposez une nouvelle version du code qui corrige le problème.
Encore une fois, il existe plusieurs variantes de solutions, mais dans tous les cas, on tentera de créer une abstraction (classe abstraite ou, idéalement, interface) commune qui sera étendue par toutes les implémentations. Ensuite, on cassera le couplage fort dans `ProgressService` en référençant l'abstraction plutôt qu'une implémentation spécifique. Finalement, on fournira à `ProgressService` l'implémentation à utiliser, de façon à ce que le service n'ait pas à se soucier de l'implémentation spécifique qui est utilisée pour faire la sauvegarde.

```java
// Abstraction de dépôt de progression
public interface ProgressRepository {
    void saveProgress(String playerId, int score, int secondsLeft);
}

// Implémentation fichier
public class FileProgressRepository implements ProgressRepository {
    public void saveProgress(String playerId, int score, int secondsLeft) {
        System.out.println("Sauvegarde en cours dans un fichier pour le joueur " + playerId + " -> score=" + score + ", temps=" + secondsLeft);
    }
}

// Implémentation base de données
public class DatabaseProgressRepository implements ProgressRepository {
    public void saveProgress(String playerId, int score, int secondsLeft) {
        System.out.println("Sauvegarde en cours dans une BD pour le joueur " + playerId + " -> score=" + score + ", temps=" + secondsLeft);
    }
}

// Service qui dépend de l'abstraction
public class ProgressService {
    private ProgressRepository repository;

    public ProgressService(ProgressRepository repository) {
        this.repository = repository;
    }

    public ProgressRepository getRepository() { return this.repository; }
    public void setRepository(ProgressRepository repository) { this.repository = repository; }

    public void save(String playerId, int score, int secondsLeft) {
        this.repository.saveProgress(playerId, score, secondsLeft);
    }
}
```
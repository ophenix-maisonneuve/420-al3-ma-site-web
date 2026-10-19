---
layout: default
title: "SOLID - Corrigé séance 1"
parent: "Principes SOLID"
nav_order: 1
has_toc: false
published: true
---

## 1. SRP - Principe de responsabilité unique

### 1.1 Pourquoi le code ci-haut enfreint le principe de responsabilité unique ?
Le code proposé présente les caractéristiques d'un ***God object*** en formation, qui est l'anti-pattern le plus souvent associé à un bris de SRP. En effet, il s'agit d'une classe qui tente de tout gérer : les énigmes, les scores, les indices et le chronomètre. Les risques sont élevés que, sans refactorisation, cette classe grossira sans arrêt au fil des ajouts de fonctionnalités, la rendant toujours plus difficile à maintenir et à tester. 

### 1.2 Proposez une nouvelle version du code qui corrige le problème

Voici une façon de corriger ce problème. Bien sûr, plusieurs solutions sont acceptables ici, mais dans tous les cas, on tentera de séparer le ***God object*** en classes plus petites dont la portée est plus limitée. En d'autres mots, on veut des classes qui ne font qu'une seule chose, mais qui la font bien.

```java
// Modèle simple pour une énigme
public class Puzzle {
    private String id;
    private boolean solved;

    public Puzzle(String id) {
        this.id = id;
        this.solved = false;
    }

    public String getId() {
        return this.id;
    }

    public boolean isSolved() {
        return this.solved;
    }

    public void setSolved(boolean solved) {
        this.solved = solved;
    }
}

// Service de gestion des énigmes
public class PuzzleService {
    public void solve(Puzzle puzzle, String answer) {
        if (answer != null && answer.length() > 0) {
            puzzle.setSolved(true);
        }
    }
}

// Service d’indices
public class HintService {
    public String provideHint(String puzzleId) {
        return "Indice pour " + puzzleId + ": regardez le tableau!";
    }
}

// Service de score
public class ScoreService {
    private int score;

    public ScoreService() {
        this.score = 0;
    }

    public int getScore() {
        return this.score;
    }

    public void addPoints(int points) {
        this.score = this.score + points;
    }

    public void penalize(int points) {
        this.score = this.score - points;
    }
}

// Service de chronomètre
public class TimerService {
    private int secondsRemaining;

    public TimerService(int seconds) {
        this.secondsRemaining = seconds;
    }

    public int getSecondsRemaining() {
        return this.secondsRemaining;
    }

    public void tick() {
        this.secondsRemaining = this.secondsRemaining - 1;
    }
}

// Orchestrateur minimal
public class GameController {

    // Attention : nous verrons plus tard une méthode plus optimale que l'instanciation directe. 
    private PuzzleService puzzleService = new PuzzleService();
    private HintService hintService = new HintService();
    private ScoreService scoreService = new ScoreService();
    private TimerService timerService = new TimerService(900);

    public void startGame() {
        // démarrage logique (aucune logique lourde ici)
    }

    public void attemptPuzzle(Puzzle puzzle, String answer) {
        this.puzzleService.solve(puzzle, answer);
        if (puzzle.isSolved()) {
            this.scoreService.addPoints(100);
        }
    }

    public String askHint(String puzzleId) {
        String hint = this.hintService.provideHint(puzzleId);
        this.scoreService.penalize(20);
        return hint;
    }

    public void tick() {
        this.timerService.tick();
    }
}
```

## 2. OCP - Principe ouvert/fermé

### 2.1. Pourquoi le code ci-haut enfreint le principe ouvert/fermé ?
Aussitôt que l'on désire ajouter un nouveau type d'énigme, par exemple une question mathématique représentée par la valeur `MATH`, on doit modifier la classe `DifficultyCalculator` en y ajoutant un nouveau `else if`. Cela pose les deux problèmes principaux suivants :
- On modifie du code qui est probablement déjà testé et validé. Ainsi, en le modifiant (possiblement à répétition), on augmente le risque d'introduire de nouveau bugs souvent appelés "régressions".
- À la longue, si on ajoute beaucoup de nouveux types d'énigmes, cette classe pourrait devenir très longue et donc de moins en moins lisible et maintenable pour une équipe de développement.

### 2.2. Proposez une nouvelle version du code qui corrige le problème.
Voici une façon de corriger ce problème. Dans tous les cas, on tentera d'éliminer les clauses `if`/`else if`/`else` par une structure qui permet un changement de comportement en ajoutant du nouveau code plutôt qu'en modifiant du code existant (d'où le nom ouvert/fermé : ouvert à l'extension, fermé à la modification).

```java
public interface Puzzle {
    String getId();
    int getTempsAlloue(); // en secondes
}

public class LogicPuzzle implements Puzzle {
    private String id;
    public LogicPuzzle(String id) {
        this.id = id;
    }

    public String getId() {
        return this.id;
    }

    public int getTempsAlloue() {
        return 420;
    }
}

public class MazePuzzle implements Puzzle {
    private String id;
    public MazePuzzle(String id) {
        this.id = id;
    }

    public String getId() {
        return this.id;
    }

    public int getTempsAlloue() {
        return 600;
    }
}

public class MathPuzzle implements Puzzle {
    private String id;
    public MathPuzzle(String id) {
        this.id = id;
    }

    public String getId() {
        return this.id;
    }

    public int getTempsAlloue() {
        return 300;
    }
}

// Utilisation
public class DifficultyCalculator {
    public int computeSecondsForPuzzle(Puzzle puzzle) {
        return puzzle.getTempsAlloue();
    }
}
```

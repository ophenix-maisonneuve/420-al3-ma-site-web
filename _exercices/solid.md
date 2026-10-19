---
layout: default
title: "Principes SOLID"
nav_order: 10
has_toc: false
published: true
---
# Exercice: Analyse et correctifs SOLID

## Objectifs

- Comprendre les principes SOLID
- Analyser des cas où ils sont enfreints et les conséquences
- Proposer des correctifs appropriés

## Contexte

On vous propose ci-bas différents extraits d'une application fictive qui implémente un jeu de type *Escape Room* en ligne. Chacun des extraits enfreint l'un des principes SOLID.

## 1. SRP - Principe de responsabilité unique

```java
public class GameManager {
    private int score;
    private int secondsRemaining;

    public GameManager() {
        this.score = 0;
        this.secondsRemaining = 900; // 15 minutes
    }

    public int getScore() {
        return this.score;
    }

    public int getSecondsRemaining() {
        return this.secondsRemaining;
    }

    // Démarrer la partie + démarrer le chronomètre + logger
    public void startGame() {
        System.out.println("Partie démarrée.");
        this.secondsRemaining = 900;
    }

    // Gérer une énigme, donner un indice, augmenter score, logger
    public void solvePuzzle(String puzzleId, String answer) {
        if (answer != null && answer.length() > 0) {
            this.score = this.score + 100;
            System.out.println("Enigme " + puzzleId + " complétée -> +100 points");
        } else {
            System.out.println("Mauvaise réponse.");
        }
    }

    // Gestion du timer (décrément + logs)
    public void tick() {
        this.secondsRemaining = this.secondsRemaining - 1;
        if (this.secondsRemaining % 60 == 0) {
            System.out.println("Temps restant: " + this.secondsRemaining + "s");
        }
        if (this.secondsRemaining <= 0) {
            System.out.println("Temps écoulé, mission ÉCHOUÉE!");
        }
    }

    // Donner un indice ET pénaliser le score
    public String giveHint(String puzzleId) {
        this.score = this.score - 20;
        return "Indice pour " + puzzleId + ": regardez le tableau!";
    }
}
```

1. Pourquoi le code ci-haut enfreint le principe de responsabilité unique ?
2. Proposez une nouvelle version du code qui corrige le problème.

## 2. OCP - Principe ouvert/fermé

```java
/**
 * Classe qui calcule le temps alloué pour une épreuve en
 * fonction du type d'épreuve.
 */
public class DifficultyCalculator {
    public int computeSecondsForPuzzle(String puzzleType) {
        if ("TEXT".equals(puzzleType)) {
            return 300;
        } else if ("LOGIC".equals(puzzleType)) {
            return 420;
        } else if ("MAZE".equals(puzzleType)) {
            return 600;
        } else {
            return 240;
        }
    }
}
```

1. Pourquoi le code ci-haut enfreint le principe ouvert/fermé ?
   1. **Indice** : Si on désirait ajouter un nouveau type d'énigme, comment devrait-on procéder ?
2. Proposez une nouvelle version du code qui corrige le problème.

## 3. LSP - Principe de substitution de Liskov

```java

// Contrat implicite : attempt() ne lance pas d'exception et retourne false si l'entrée est invalide (null)
public class PuzzleBase {
    public boolean attempt(String input) {
        if (input == null) {
            return false;
        }
        return "START".equals(input);
    }
}

public class LaserMazePuzzle extends PuzzleBase {
    @Override
    public boolean attempt(String input) {
        if (input == null) {
            throw new IllegalArgumentException("LaserMaze requires a START or RUN command.");
        }
        if (!("START".equals(input) || "RUN".equals(input))) {
            throw new UnsupportedOperationException("Invalid command for LaserMaze.");
        }

        // exécuter de la logique métier ici, au besoin

        return true;
    }
}

// Code client
public class PuzzleRunner {
    public boolean run(PuzzleBase puzzle, String input) {
        return puzzle.attempt(input); // suppose aucune exception
    }
}
```

1. Que se passera-t-il si l'entrée (`input`) est `null` dans les cas suivants :
   1. Si `puzzle` est de type `PuzzleBase` ?
   2. Si `puzzle` est de type `LaserMazePuzzle` ?
2. En fonction de vos réponses ci-haut, le principe de substitution de Liskov est-il respecté ? Pourquoi ?
3. Si le principe n'est pas respecté, proposez une nouvelle version du code qui corrige le problème.

## 4. ISP - Principe de ségrégation des interfaces

```java
/**
 * Interface qui définit une console de jeu utilisable pour jouer au escape room
 * Par exemple : terminal de texte, PC Windows, iPhone, etc
 */
public interface GameConsole {
    void displayText(String text);
    void playSound(String soundFile);
    void vibrate(int intensity);
    void showMap(String roomId);
    void recordVideo(String filePath);
}

/**
 * Implémentation la plus basique d'une console de jeu : un terminal texte
 */
public class TextTerminal implements GameConsole {
    public void displayText(String text) {
        System.out.println(text);
    }

    public void playSound(String soundFile) {
        throw new UnsupportedOperationException("Le son n'est pas pris en charge.");
    }

    public void vibrate(int intensity) {
        throw new UnsupportedOperationException("La vibration n'est pas prise en charge.");
    }

    public void showMap(String roomId) {
        throw new UnsupportedOperationException("Les maps complexes ne sont pas prises en charge.");
    }

    public void recordVideo(String filePath) {
        throw new UnsupportedOperationException("La vidéo n'est pas prise en charge.");
    }
}
```

1. Pourquoi le code ci-haut enfreint le principe de ségrégation des interfaces ?
   1. **Indice** : Est-il réellement utile d'implémenter les méthodes `playSound`, `vibrate`, `showMap` et `recordVideo` pour un terminal de texte ?
2. Proposez une nouvelle version du code qui corrige le problème.

## 5. DIP - Principe d'inversion des dépendances

```java
public class FileProgressRepository {
    public void saveProgress(String playerId, int score, int secondsLeft) {
        System.out.println("Sauvegarde en cours dans un fichier pour le joueur " + playerId + " -> score=" + score + ", temps=" + secondsLeft);
    }
}

public class ProgressService {
    private FileProgressRepository repository;

    public ProgressService() {
        this.repository = new FileProgressRepository();
    }

    public void save(String playerId, int score, int secondsLeft) {
        this.repository.saveProgress(playerId, score, secondsLeft);
    }
}

```

1. Supposez que l'on désire maintenant proposer la sauvegarde dans une base de données plutôt que dans un fichier.
   1. Avec le code actuel, quelle(s) modification(s) serait(ent) nécessaire(s) pour prendre en charge ce nouveau type de sauvegarde ?
   2. Cette modification enfreindrait-elle l'un des 4 autres principes (S-O-L-I) ? Pourquoi ?
   3. Avec le code actuel, serait-il possible de prendre en charge à la fois la sauvegarde dans un fichier et la sauvegarde en BD sans changer le code de `ProgressService` ? Pourquoi ?
2. En fonction de vos réponses aux questions précédentes, croyez-vous que le code ci-haut respecte le principe d'inversion des dépendances ?
3. Si le code ne respecte pas DIP, proposez une nouvelle version du code qui corrige le problème.


## 6. Bonus - Identifier les infractions SOLID
Analysez le code ci-bas. Au besoin, vous pouvez le copier dans un fichier `GestionnaireAgenceVoyages.java` dans votre IDE de choix.

```java
package ca.qc.cmaisonneuve.amp.solid;

import java.util.ArrayList;
import java.util.List;

public class GestionnaireAgenceVoyages {

    private boolean hauteSaison = false;
    private double fraisAeroport = 35.0;
    public static double fondDeCaisse = 1000.0;

    private final List<Itineraire> itineraires = new ArrayList<>();

    public GestionnaireAgenceVoyages() {
        System.out.println("[GUICHET] Bienvenue chez J'ai mon voyage !");
    }

    public void ajouterReservation(Itineraire itineraire) {
        itineraires.add(itineraire);
    }

    public void traiterReservations() {
        double revenuTotal = 0.0;
        for (Itineraire it : itineraires) {
            System.out.println("[GUICHET] Traitement : " + it.type() + " " + it.origine() + " >> " + it.destination());
            if (!villeSupportee(it.origine())) {
                System.out.println("[GUICHET] Ville d'origine invalide.");
                continue;
            }
            double prix;
            switch (it.type()) {
                case "INTERIEUR":
                    prix = prixInterieur(it);
                    break;
                case "INTERNATIONAL":
                    prix = prixInternational(it);
                    break;
                case "NOLISE":
                    prix = prixNolise(it);
                    break;
                default:
                    System.out.println("Type inconnu");
                    continue;
            }
            prix = prix + fraisAeroport;
            fondDeCaisse = fondDeCaisse + (prix * 0.05);
            String libelle;
            if ("INTERIEUR".equals(it.type())) {
                libelle = "Billet interieur " + it.origine() + " >> " + it.destination();
            } else if ("INTERNATIONAL".equals(it.type())) {
                libelle = "Billet international " + it.origine() + " >> " + it.destination();
            } else if ("NOLISE".equals(it.type())) {
                libelle = "Billet nolise " + it.origine() + " >> " + it.destination();
            } else {
                libelle = "Billet inconnu";
            }

            System.out.println("Billet emis : " + libelle + " (" + prix + " $)");
            appendCsv("reservations.csv", it.type() + "," + it.origine() + "," + it.destination() + "," + prix);
            revenuTotal += prix;
        }
        appendCsv("reservations.csv", "REVENU_TOTAL," + revenuTotal);
        System.out.println("Fonds restants : " + fondDeCaisse);
    }

    private double prixInterieur(Itineraire it) {
        return 200.0 + (it.passagers() - 1) * 90.0;
    }

    private double prixInternational(Itineraire it) {
        return 650.0 + (hauteSaison ? 40.0 : 0);
    }

    private double prixNolise(Itineraire it) {
        return 1200.0;
    }

    private void appendCsv(String f, String l) {
        System.out.println("[CSV] " + f + " << " + l);
    }

    private boolean villeSupportee(String v) {
        System.out.println("[VALID] " + v);
        return v != null && v.length() > 1;
    }

    private static record Itineraire(String type, int passagers, String origine, String destination) {}

    public static void main(String[] args) {
        GestionnaireAgenceVoyages agence = new GestionnaireAgenceVoyages();
        agence.ajouterReservation(new Itineraire("INTERIEUR", 2, "Montreal", "Vancouver"));
        agence.ajouterReservation(new Itineraire("INTERNATIONAL", 1, "Montreal", "Paris"));
        agence.traiterReservations();
    }
}
```

1. Identifiez chacune des infractions SOLID présentes dans le code.
1. En vos mots, expliquez les refactorisations que vous pourriez effectuer pour rendre le code conforme aux principes SOLID.
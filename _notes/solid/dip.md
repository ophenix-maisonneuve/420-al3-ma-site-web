---
layout: default
title: "DIP - Inversion des dépendances"
parent: "Principes SOLID"
nav_order: 5
published: true
---

# DIP — Inversion des dépendances (*Dependency Inversion Principle*)

Les modules de haut niveau ne doivent pas dépendre de modules de bas niveau : tous deux doivent **dépendre d’abstractions** (interfaces). Les détails (implémentations concrètes) doivent aussi **dépendre d’abstractions**, et non l’inverse.

## Pourquoi ?
Le code métier (*business logic*) **couplé** à des composants techniques (base de données, API, fichiers) est **difficile à tester** et très **rigide** face aux changements d’implémentation.

## Définition
Le **DIP** sépare **politiques** (règles métier ou *business logic*) et **détails** (implémentations techniques) en **inversant** la dépendance : les modules haut niveau et bas niveau **dépendent d’une abstraction** commune. Concrètement, on **déclare** des interfaces au niveau métier et on **utilise** les implémentations techniques en passant toujours par l'abstraction et non par l'implémentation spécifique. On gagne en **testabilité** (mocks), en **flexibilité** (remplacer MySQL par mémoire ou HTTP), et on **préserve** l’**OCP** en ajoutant des implémentations sans modifier le service.

## Exemple qui ne respecte pas DIP
```java
public class MySQLCommandeRepository {
    public void sauvegarder(String commande) {
        System.out.println("Sauvegarde MySQL: " + commande);
    }
}

public class ServiceCommande {
    private MySQLCommandeRepository repository;
    public ServiceCommande() {
        this.repository = new MySQLCommandeRepository();
    }

    public void traiter(String commande) {
        this.repository.sauvegarder(commande);
    }
}
```

{: .warning}
Ici, `ServiceCommande` **dépend** d’une **classe concrète** (`MySQLCommandeRepository`). Le service est **verrouillé** sur MySQL et **difficile à tester** (pas d’abstraction à mocker).

## Exemple corrigé conforme au DIP
```java
public interface CommandeRepository {
    void sauvegarder(String commande);
}

public class MySQLCommandeRepository implements CommandeRepository {
    public void sauvegarder(String commande) {
        System.out.println("Sauvegarde MySQL: " + commande);
    }
}

public class MemoireCommandeRepository implements CommandeRepository {
    public void sauvegarder(String commande) {
        System.out.println("Sauvegarde en mémoire: " + commande);
    }
}

public class ServiceCommande {
    private CommandeRepository repository;

    public ServiceCommande(CommandeRepository repository) {
        this.repository = repository;
    }

    public CommandeRepository getRepository() {
        return this.repository;
    }
    public void setRepository(CommandeRepository repository) {
        this.repository = repository;
    }

    public void traiter(String commande) {
        this.repository.sauvegarder(commande);
    }
}
```

{: .highlight}
> Le service dépend maintenant **d’une abstraction** (`CommandeRepository`). On **injecte** l’implémentation voulue (MySQL, mémoire, etc.), ce qui **facilite** les tests et **évite** le couplage aux détails.

## Liens utiles
- [https://en.wikipedia.org/wiki/Dependency_inversion_principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
- [https://blog.logrocket.com/dependency-inversion-principle/](https://blog.logrocket.com/dependency-inversion-principle/)
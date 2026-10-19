---
layout: default
title: "SRP - Responsabilité unique"
parent: "Principes SOLID"
nav_order: 1
published: true
---

# SRP — Responsabilité unique (*Single Responsibility Principle*)

Une classe ne doit avoir **qu’une seule raison de changer** : elle doit être responsable d’une seule chose.

## Pourquoi ?
Quand une classe **porte plusieurs responsabilités** (accès aux données, formatage, journalisation, export), chaque changement augmente le risque de **régression**, **alourdit** les tests et **complique** la compréhension.

## Définition
Le SRP vise la **cohésion forte** : toutes les méthodes d’une classe doivent servir un **même but**. En pratique, on découpe une logique complexe en **rôles spécialisés** (récupération des données, logique métier, présentation/formatage, persistance/export), ce qui rend les **tests unitaires** plus simples, réduit les **dépendances transversales**, et améliore la **réutilisabilité**. Respecter le SRP clarifie aussi les **responsabilités organisationnelles** : si une règle métier évolue, la classe concernée a une **seule raison** de changer et l’impact sur le code est généralement plus limité. Combiné avec l’**ISP** (pour éviter les interfaces trop générales) et le **DIP** (pour dépendre d’abstractions), le SRP est une bonne défense contre les *code smells* et les effets domino.

## Exemple qui ne respecte pas SRP
```java
public class RapportUtilisateur {
    private int idUtilisateur;
    private String nomUtilisateur;

    public RapportUtilisateur(int idUtilisateur, String nomUtilisateur) {
        this.idUtilisateur = idUtilisateur;
        this.nomUtilisateur = nomUtilisateur;
    }

    public int getIdUtilisateur() {
         return this.idUtilisateur;
    }
    
    public void setIdUtilisateur(int idUtilisateur) {
        this.idUtilisateur = idUtilisateur;
    }
    
    public String getNomUtilisateur() {
        return this.nomUtilisateur;
    }
    
    public void setNomUtilisateur(String nomUtilisateur) {
        this.nomUtilisateur = nomUtilisateur;
    }

    // Mélange de responsabilités : accès aux données, formatage, export
    public String chargerDonneesDepuisBase() {
        return "Données pour " + this.nomUtilisateur;
    }

    public String formatterRapport(String donnees) {
        return "=== RAPPORT ===" + donnees;
    }

    public void exporterVersFichier(String contenu) {
        System.out.println("Export fichier: " + contenu);
    }

    public void genererRapportComplet() {
        String donnees = this.chargerDonneesDepuisBase();
        String rapport = this.formatterRapport(donnees);
        this.exporterVersFichier(rapport);
    }
}
```

{: .warning}
> Ici, la classe **mélange trois raisons de changer** : l’accès aux données, le **formatage** et l’**export**. Une modification de la base, du format de sortie, ou du mécanisme d’export **entraîne des changements** dans la même classe, augmentant le **couplage** et les **risques**.

## Exemple corrigé conforme au SRP
```java
public class UtilisateurRepository {
    public String chargerDonnees(int idUtilisateur) {
        return "Données pour utilisateur #" + idUtilisateur;
    }
}

public class RapportFormatter {
    public String formatter(String donnees, String nomUtilisateur) {
        return "=== RAPPORT UTILISATEUR === Nom: " + nomUtilisateur + donnees;
    }
}

public class RapportExporter {
    public void exporter(String contenu) {
        System.out.println("Export fichier: " + contenu);
    }
}

public class RapportService {
    private UtilisateurRepository repository;
    private RapportFormatter formatter;
    private RapportExporter exporter;

    public RapportService(UtilisateurRepository repository, RapportFormatter formatter, RapportExporter exporter) {
        this.repository = repository;
        this.formatter = formatter;
        this.exporter = exporter;
    }

    public void genererRapport(int idUtilisateur, String nomUtilisateur) {
        String donnees = this.repository.chargerDonnees(idUtilisateur);
        String rapport = this.formatter.formatter(donnees, nomUtilisateur);
        this.exporter.exporter(rapport);
    }
}
```

{: .highlight}
> Chaque classe a maintenant **une responsabilité unique** (données, formatage, export). Les changements futurs seront **localisés**, rendant le code **testable** et **extensible** sans modifier les autres rôles.

## Liens utiles
- [https://en.wikipedia.org/wiki/Single-responsibility_principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)
- [https://www.baeldung.com/solid-principles](https://www.baeldung.com/solid-principles)

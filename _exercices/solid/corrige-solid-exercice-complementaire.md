---
layout: default
title: "SOLID - Corrigé exercice complémentaire"
parent: "Principes SOLID"
nav_order: 3
has_toc: false
published: true
---

## 6. Bonus - Identifier les infractions SOLID

### Résumé des infractions présentes dans le code
- **SRP** : orchestration, tarification, persistance (export en CSV) et configuration dans la même classe. Attention au ***God object*** !
- **OCP** : `switch/case` pour le calcul du prix, `if/else` fermés pour le formattage du libellé de l'itinéraire, gestion de la petite caisse directement dans le code (tout changement de méthode de calcul ou de prix nécessitera un changement dans le code).
- **DIP** : aucune abstraction, toute la logique est en dur un peu partout dans le flux.


### Étapes proposées de refactorisation
Ici, il n'existe pas une réponse unique. De façon générale, on voudra d'abord régler les problèmes fondamentaux (le premières lettres de l'acronyme SOLID) avant de s'attaquer aux principes plus complexes, par exemple l'inversion de dépendances (*DIP*). En procédant ainsi de façon itérative, on évitera de se sentir submergé par le problème. On décompose la montagne en petits plateaux que l'on gravit un à la fois.

Une approche itérative pourrait donc ressembler à ceci:

1. Régler **SRP** : Décomposer la ***God class*** `GestionnaireAgenceVoyages` en classes plus petites avec des responsabilités ciblées, par exemple...
   - `ServicePrix` : classe responsable du calcul du prix des billets
      - *Cette classe contiendrait la logique présentement contenue dans le `switch/case` et la variante `hauteSaison`, ainsi que les frais d'aéroport*.
   - `ServiceBillet` : classe responsable de la composition du libellé des billets
      - *Cette classe contiendrait la logique présentement contenue dans la série de `if/else` qui compose le libellé.*
   - `ServiceExport` : classe responsable d'écrire la transaction dans un fichier CSV.
      - On peut, pour l'instant, simplement déplacer la logique de la méthode `appendToCsv` existante dans cette classe.
   - `ServicePetiteCaisse` : classe responsable de gérer la petite caisse.
   - `GestionnaireAgenceVoyages` : classe existante, conserve uniquement la responsabilité d'orchestrer les appels aux classes spécialisées créées précédemment.

**Exemple pour `ServicePrix` et `ServiceExport` et `GestionnaireAgenceVoyages` après les modifications ci-haut.**
```java
public class ServicePrix {

    private double fraisFixes;

    public ServicePrix(double fraisFixes) {
        this.fraisFixes = fraisFixes;
    }

    public double calculer(Itineraire it, boolean hauteSaison) {
            return fraisFixes + switch (it.type()) { 
                case "INTERIEUR" -> prixInterieur(it);
                case "INTERNATIONAL" -> prixInternational(it, hauteSaison);
                case "NOLISE" -> prixNolise(it);
                default -> throw new IllegalArgumentException("Type de vol invalide :" + it.type());
            };
    }

    private double prixInterieur(Itineraire it) {
        return 200.0 + (it.passagers() - 1) * 90.0;
    }

    private double prixInternational(Itineraire it, boolean hauteSaison) {
        return 650.0 + (hauteSaison ? 40.0 : 0);
    }

    private double prixNolise(Itineraire it) {
        return 1200.0;
    }

}
```

```java
public class ServiceExport {

    public void appendCsv(String fichier, String ligne) {
        System.out.println("[CSV] " + fichier + " << " + ligne);
    }
}
```

```java
public void traiterReservations() {
    double revenuTotal = 0.0;
    ServiceExport serviceExport = new ServiceExport();
    ServicePrix servicePrix = new ServicePrix(fraisAeroport);
    ServiceBillets serviceBillets = new ServiceBillets();
    for (Itineraire it : itineraires) {
        System.out.println("[GUICHET] Traitement : " + it.type() + " " + it.origine() + " >> " + it.destination());
        if (!villeSupportee(it.origine())) {
            System.out.println("[GUICHET] Ville d'origine invalide.");
            continue;
        }
        double prix = servicePrix.calculer(it, hauteSaison);
        String libelle = serviceBillets.creerLibelle(it);
        fondDeCaisse = fondDeCaisse + (prix * 0.05);

        System.out.println("Billet emis : " + libelle + " (" + prix + " $)");
        serviceExport.appendCsv("reservations.csv", it.type() + "," + it.origine() + "," + it.destination() + "," + prix);
        revenuTotal += prix;
    }
    serviceExport.appendCsv("reservations.csv", "REVENU_TOTAL," + revenuTotal);
    System.out.println("Fonds restants : " + fondDeCaisse);
}
```

2. Régler **OCP** : Identifier les points d'extension probables et créer des interfaces...
   1. Pour se débarasser du `switch/case` qui a été déplacé dans `CalculateurPrix`
      - Transformer `ServicePrix` en interface avec une méthode `int calculerPrix(Itineraire it, boolean hauteSaison)`
      - Créer une implémentation de `ServicePrix` pour chaque type de vol, par exemple :
         - `ServicePrixInterieur` : calcul des prix pour les vols intérieurs
         - `ServicePrixInternational` : calcul des prix pour les vols internationaux
         - etc 
      - Déplacer la logique de calcul des prix (qui était précédemment dans le `switch/case`) dans les implémentations appropriées
   1. Répéter l'étape précédente pour le `ServiceBillet` afin d'obtenir un générateur de libellé pour chaque implémentation requise.
      - *Il est possible que nous n'ayons pas besoin d'une implémentation par type de vol. En effet, la logique spécifique à chaque cas est très simple et peut très bien être généralisée en une seule implémentation `ServiceBilletsSimple`*
   1. Répéter encore une fois les étapes précédentes pour `ServiceExport` afin de permettre, éventuellement, un export dans un autre format que CSV. Pour le moment, nous aurions:
      - `ServiceExport` : interface contenant les méthodes `void add(Itineraire it, double prix)` (ajout d'un itinéraire et de son prix au rapport) et `void export(String fichier)` (export du rapport).
      - `ServiceExportCSV` : implémentation de `ServiceExport` qui effectue l'export en format CSV
   1. Il est possible que l'on juge que `ServicePetiteCaisse` n'est pas vraiment un point d'extension requis, surtout si l'on juge que les méthodes de calcul sont peu susceptibles de changer.


3. Après avoir effectué les étapes précédentes, vous aurez grandement amélioré le respect des principes **SRP** et **OCP** du code. Cependant, vous aurez fort probablement des instructions ressemblant à celles-ci dans votre code:

```java
ServiceExport serviceExport = new ServiceExportCsv();
ServicePrix servicePrix = new ServicePrixInterieur(fraisAeroport);
ServiceBillets serviceBillets = new ServiceBilletsSimple();
```

Ces instanciations ne respectent pas le principe d'inversion de dépendances (**DIP**), car notre classe de haut-niveau `GestionnaireAgenceVoyages` dépend d'implémentations concrètes des classes techniques. Pour corriger:

   1. `ServiceExport` et `ServicePrix` sont déjà des interfaces. Il est donc possible de ne dépendre que du contrat et de recevoir une implémentation spécifique, par exemple via l'injection au constructeur.

   1. `ServicePetiteCaisse` n'a cependant pas d'abstraction. Nous pouvons donc transformer cette classe en une interface qui n'aura qu'une seule implémentation, qui sera également passée au constructeur. Le code final pourrait ressembler à:

   ```java
    ...
    private final List<ServicePrix> servicesPrix = new ArrayList<>();
    private final ServicePetiteCaisse servicePetiteCaisse;
    private final ServiceExport serviceExport;
    private final ServiceBillets serviceBillets;

    public GestionnaireAgenceVoyages(List<ServicePrix> servicePrix, ServiceExport serviceExport, ServicePetiteCaisse servicePetiteCaisse, ServiceBillets serviceBillets) {
        this.servicesPrix.addAll(servicePrix);
        this.serviceExport = serviceExport;
        this.servicePetiteCaisse = servicePetiteCaisse;
        this.serviceBillets = serviceBillets;
        
        System.out.println("[GUICHET] Bienvenue chez J'ai mon voyage !");
    }

    ...

   ```
   ```java
    public static void main(String[] args) {
        // ce serait encore mieux si on lisait ces valeur au démarrage ou dans une
        // config, mais bon...
        double fraisAeroport = 35.0;
        double soldeInitialPetiteCaisse = 1000.0;

        List<ServicePrix> servicesPrix = List.of(new ServicePrixInterieur(fraisAeroport),
                new ServicePrixInternational(fraisAeroport),
                new ServicePrixNolise(fraisAeroport));
        ServiceExport serviceExport = new ServiceExportCsv();
        ServicePetiteCaisse servicePetiteCaisse = new ServicePetiteCaisseRegulier(soldeInitialPetiteCaisse);
        ServiceBillets serviceBillets = new ServiceBilletsSimple();

        GestionnaireAgenceVoyages agence = new GestionnaireAgenceVoyages(servicesPrix, serviceExport,
                servicePetiteCaisse, serviceBillets);
        agence.ajouterReservation(new Itineraire("INTERIEUR", 2, "Montreal", "Vancouver"));
        agence.ajouterReservation(new Itineraire("INTERNATIONAL", 1, "Montreal", "Paris"));
        agence.traiterReservations();
    }
   ```

### Pistes d'améliorations supplémentaires

Félicitations, vous avez **grandement** amélioré la qualité du code du gestionnaire de l'agence de voyages. Si vous désirez pousser l'exercice plus loin, quelques autres éléments, plus mineurs, peuvent aussi être améliorés:

#### 1. Validation de la ville

La méthode `private boolean villeSupportee(String ville)` ne fait qu'une validation minimale; elle vérifie que la ville est une chaîne de caractère non-vide. Si on voulait améliorer la validation, on pourrait faire une implémentation plus robuste qui, par exemple, lit à partir d'un fichier de configuration toutes les villes pour lesquelles l'agence offre des départs ou des arrivées. Ainsi, toute ville entrée qui n'est pas prise en charge serait refusée, et l'ajout ou le retrait d'une ville ne demanderait que la modification d'un fichier de configuration (ou toute autre source externe, comme une base de données).

#### 2. Journalisation (*logs*)

Les journaux (*logs*) de l'application sont écrits à l'aide de `System.out.println`. Dans une application de production, il serait préférable d'utiliser un véritable *framework* de logs, comme *SFL4J* en Java (il en existe aussi plusieurs autres).
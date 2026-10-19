---
layout: default
title: "Structures de données – semaine 4"
parent: "Structures de données"
nav_order: 4
published: false
---

# Exercice : Structures de données – *Maps* (messagerie des joueurs)

## Contexte
Bienvenue dans **Le Lobby des Braves** ! Cette semaine, vous devez enrichir votre application en ajoutant une **messagerie** entre joueurs. Chaque **joueur** sera la **clé** d’une *table associative* et la **valeur** sera une **liste de messages** qui lui sont destinés.

Nous allons explorer trois implémentations de `Map` :
- `TreeMap` pour un **ordre trié** des destinataires (transition naturelle après `TreeSet`).
- `HashMap` pour des **performances optimales** sans ordre garanti.
- `ConcurrentHashMap` pour les cas nécessitant la **concurrence** (*thread‑safe*).

## Objectifs
- Concevoir une *table associative* `Map<Joueur, List<Message>>` pour la messagerie.
- Implémenter un gestionnaire avec `TreeMap` (clés triées, navigation `higherKey`, `lowerKey`, `subMap`).
- Implémenter une variante avec `HashMap` (performance amortie O(1), ordre non garanti).
- Implémenter une variante *thread‑safe* avec `ConcurrentHashMap` (opérations atomiques, itérateurs faiblement cohérents).
- Discuter des **complexités**, des **pièges** (`equals`/`hashCode` de `Joueur`, ordre des clés), et des **choix** de structure.

---

## Étapes préparatoires
### 1. Clonez (ou mettez à jour) le dépôt
```bash
git clone git@github.com:ophenix-420-930-ma-24636/lobby-braves.git
```
Ou :
```bash
git pull
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/lobby-braves-1.0-SNAPSHOT.jar
```

### 3. Créez l’interface `GestionnaireMessagerie`
```java
public interface GestionnaireMessagerie {
    void envoyerMessage(Joueur destinataire, Message message);
    java.util.List<Message> messagesDe(Joueur destinataire);
    void supprimerMessagesDe(Joueur destinataire);
    void afficher();
}
```
{: .highlight}
> Cette interface sera implémentée par les trois gestionnaires (`TreeMap`, `HashMap`, `ConcurrentHashMap`).

Créez la classe `Message` :
```java
public class Message {
    private String texte;
    private java.time.Instant horodatage;

    public Message(String texte, java.time.Instant horodatage) {
        this.texte = texte;
        this.horodatage = horodatage;
    }

    public String getTexte() { return this.texte; }
    public void setTexte(String texte) { this.texte = texte; }

    public java.time.Instant getHorodatage() { return this.horodatage; }
    public void setHorodatage(java.time.Instant horodatage) { this.horodatage = horodatage; }

    @Override
    public String toString() {
        return "Message{" +
               "texte='" + this.texte + '\'' +
               ", horodatage=" + this.horodatage +
               '}';
    }
}
```

---

## 1. Messagerie ordonnée avec TreeMap
### 1.1. Créez `GestionnaireMessagerieTreeMap`
Implémentez l’interface avec un champ :
```java
public class GestionnaireMessagerieTreeMap implements GestionnaireMessagerie {
    private final java.util.NavigableMap<Joueur, java.util.List<Message>> messages;

    public GestionnaireMessagerieTreeMap() {
        this.messages = new java.util.TreeMap<>(new java.util.Comparator<Joueur>() {
            @Override
            public int compare(Joueur j1, Joueur j2) {
                return j1.getPseudo().compareTo(j2.getPseudo());
            }
        });
    }

    @Override
    public void envoyerMessage(Joueur destinataire, Message message) {
        java.util.List<Message> liste = this.messages.get(destinataire);
        if (liste == null) {
            liste = new java.util.ArrayList<>();
            this.messages.put(destinataire, liste);
        }
        liste.add(message);
    }

    @Override
    public java.util.List<Message> messagesDe(Joueur destinataire) {
        java.util.List<Message> liste = this.messages.get(destinataire);
        if (liste == null) {
            return java.util.Collections.emptyList();
        }
        return java.util.Collections.unmodifiableList(liste);
    }

    @Override
    public void supprimerMessagesDe(Joueur destinataire) {
        this.messages.remove(destinataire);
    }

    @Override
    public void afficher() {
        for (java.util.Map.Entry<Joueur, java.util.List<Message>> e : this.messages.entrySet()) {
            System.out.println("Destinataire: " + e.getKey());
            for (Message m : e.getValue()) {
                System.out.println("  - " + m);
            }
        }
    }
}
```

### 1.2. Navigation des destinataires
- Récupérez le **premier** et le **dernier** destinataire (`firstKey`, `lastKey`).
- Trouvez le **suivant** et le **précédent** (`higherKey`, `lowerKey`).
- Créez une **fenêtre** de destinataires avec `subMap(from, true, to, true)`.

{: .astuce}
> `TreeMap` implémente `NavigableMap` : vous pouvez aussi parcourir en ordre **décroissant** via `descendingMap()`.

---

## 2. Messagerie avec HashMap
### 2.1. Créez `GestionnaireMessagerieHashMap`
Implémentez l’interface avec un champ :
```java
private final java.util.Map<Joueur, java.util.List<Message>> messages = new java.util.HashMap<>();
```
- Pourquoi `HashMap` est-elle **O(1)** en moyenne pour `get` et `put` ?
- Que se passe-t-il en cas de **collisions** ? Expliquez la **treeification** des bins en Java 8+.

### 2.2. Améliorez avec `computeIfAbsent`
Réécrivez `envoyerMessage` :
```java
java.util.List<Message> liste = this.messages.computeIfAbsent(
    destinataire,
    j -> new java.util.ArrayList<>()
);
liste.add(message);
```

---

## 3. Messagerie concurrente avec ConcurrentHashMap
### 3.1. Créez `GestionnaireMessagerieConcurrent`
Utilisez `computeIfAbsent` pour des insertions atomiques :
```java
private final java.util.concurrent.ConcurrentHashMap<Joueur, java.util.List<Message>> messages =
    new java.util.concurrent.ConcurrentHashMap<>();
```
- Pourquoi `ConcurrentHashMap` est-elle **préférée** à `HashMap` en multithread ?
- Quels sont les **pièges** à éviter avec `computeIfAbsent` ?

---

## 4. Tests et validation
- Générez 50 joueurs, envoyez des messages.
- Vérifiez l’ordre avec `TreeMap`, la performance avec `HashMap`, et la sécurité avec `ConcurrentHashMap`.

---

## 5. Bonus
- Limiter la taille des boîtes de réception.
- Export CSV.
- Index par pseudo avec `TreeMap<String, Joueur>`.

---

## Questions de réflexion
- Pourquoi la clé `Joueur` doit-elle avoir des `equals`/`hashCode` **stables** ?
- Dans quels cas `TreeMap` est-il **préférable** à `HashMap` pour la messagerie ?
- Pourquoi `ConcurrentHashMap` n’offre pas de **verrou global** et quelles en sont les **implications** ?

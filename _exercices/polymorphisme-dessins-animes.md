---
layout: default
title: "Polymorphisme en Java"
nav_order: 3
has_toc: false
published: true
---
# Exercice : Polymorphisme en Java

## Objectif

Pratiquer le polymorphisme en Java à l'aide d'une interface, d'une classe abstraite et de sous-classes concrètes.

## Contexte

Vous allez modéliser des personnages de dessins animés célèbres : Lisa Simpson, Eric Cartman et Stewie Griffin.

## Étapes

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/poly-dessins-animes.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/poly-dessins-animes.git
```

### 2. Créez une classe abstraite `AbstractPersonnage` qui implémente `PersonnageAnime`

- Ajoutez un champ `nom` privé.
- Ajoutez un constructeur qui prend en paramètre le nom du personnage (`String`)
- Implémentez la méthode `getName()` dans la classe abstraite.
  - Cette méthode doit retourner le nom du personnage.
- Implémentez la méthode `parler()` dans la classe abstraite.
  - Cette méthode doit écrire sur la console `Bonjour, je suis {nom du personnage}!`
- Laissez la méthode `agir()` non implémentées (abstraite).

### 3. Créez trois classes concrètes qui étendent `AbstractPersonnage`

- `LisaSimpson`
- `EricCartman`
- `StewieGriffin`

Implémentez la méthode `agir()` dans chaque classe avec un comportement propre au personnage. Si vous les connaissez moins:

<details markdown="1">
<summary markdown="span">Lisa Simpson</summary>

![Lisa](../assets/images/lisa.png)

Intellectuelle, idéaliste, passionée par la justice sociale, la musique et l'apprentissage.
</details>

<details markdown="1">
<summary markdown="span">Eric Cartman</summary>

![Cartman](../assets/images/cartman.png)

Manipulateur, égocentrique, provocateur, n'hésite pas à insulter ses camarades de classe et à exiger qu'ils respectent son autorité.
</details>

<details markdown="1">
<summary markdown="span">Stewie Griffin</summary>

![Stewie](../assets/images/stewie.png)

Génie machiavélique avec des ambitions de domination mondiale, mais aussi un goût pour l’humour sarcastique et la sophistication. Accessoirement: c'est aussi un bébé.
</details>

### 4. Dans une classe `Main`, créez une liste de `PersonnageAnime` contenant les trois personnages

- Créez une méthode ```public static void main(String[] args)```
- Dans cette méthode, parcourez la liste et appelez les méthodes `parler()` et `agir()` sur chaque élément.

### 5. Interaction entre personnages

Ajoutez une méthode `interagirAvec(PersonnageAnime autre)` dans l'interface `PersonnageAnime`. Cette méthode doit permettre à un personnage d'interagir avec un autre personnage en affichant un message personnalisé.

#### Exemple :

```java
public void interagirAvec(PersonnageAnime autre) {
    System.out.println(this.getNom() + " interagit avec " + autre.getNom());
}
```

Implémentez cette méthode dans chaque sous-classe (`LisaSimpson`, `EricCartman`, `StewieGriffin`) avec un message spécifique à leur personnalité.

Testez ensuite ces interactions dans une boucle où chaque personnage interagit avec les autres.

### 6. Ajout d'un personnage muet

Nous allons maintenant ajouter un 4e personnage: Maggie Simpson. Comme elle ne parle jamais, on désire qu'une exception soit lancée lorsque l'on appelle sa méthode `parler()`.

- Créez votre propre classe d'exception appelée `PersonnageMuetException`

#### Exemple:
```java
public class PersonnageMuetException extends Exception {

    private final PersonnageAnime personnage;

    public PersonnageMuetException(PersonnageAnime personnage) {
        this.personnage = personnage;
    }

    @Override
    public String getMessage() {
        return "%s ne parle pas!".formatted(personnage.getNom());
    }
}
```

- Créez une nouvelle classe `MaggieSimpson` qui étend `AbstractPersonnage`.
- Que se passe-t-il si vous tentez de faire lancer une `PersonnageMuetException` à la méthode `parler()`? Pourquoi?

#### Exemple
```java
public void parler() throws PersonnageMuetException {
  //
}
```

- Si vous modifiez la classe `PersonnageMuetException` pour qu'elle étende plutôt `RuntimeException`, observez-vous un changement de comportement? Pourquoi?


---
layout: default
title: "Annotations"
parent: "Concepts de programmation orientée objet"
nav_order: 10
---

# Annotations

Les annotations en Java sont des **métadonnées** ajoutées au code source. Elles permettent de fournir des informations supplémentaires au compilateur, aux outils de développement ou à l'exécution du programme.

---

## Qu'est-ce qu'une annotation ?

Une annotation est une **instruction spéciale** précédée du symbole `@`. Elle peut être appliquée à des classes, méthodes, champs, paramètres, etc.

```java
@Override
public String toString() {
    return "Exemple";
}
```

---

## Cas d'utilisation

Les annotations servent toujours à ajouter des métadonnées, donc de l'information supplémentaire, au code. Ces métadonnées peuvent ensuite être utilisées pour effectuer les opérations suivantes :

- **Vérification par le compilateur** (`@Override`, `@Deprecated`)
- **Configuration** (ex. `@Entity`, `@Table` en JPA)
- **Génération de code** (ex.: `@Getter` / `@Setter` de [Lombok](../notes/lombok))
- **Injection de dépendances** (`@Autowired` en Spring)
- **Documentation** (`@SuppressWarnings`)

---

## Traitement des annotations

Les annotations en Java peuvent être **informatives** (sans traitement), ou être **traitées automatiquement** soit **à la compilation**, soit **à l'exécution**.

Lorsqu'elles sont traitées **à la compilation**, c’est généralement pour **générer du code automatiquement**. Ce traitement est effectué par des **annotation processors** — des classes spécialisées qui analysent les annotations et produisent du code ou des fichiers avant que le compilateur Java ne transforme le tout en bytecode.

Ces processors utilisent l’API `javax.annotation.processing` et sont à la base d’outils populaires comme **Lombok**, **MapStruct**, ou encore des générateurs de code personnalisés.


### Exemple de processeur d'annotations (*annotation processor*) :
```java
@SupportedAnnotationTypes("com.exemple.MonAnnotation")
@SupportedSourceVersion(SourceVersion.RELEASE_8)
public class MonProcessor extends AbstractProcessor {
    // traitement personnalisé
}
```

---

## Annotations courantes

| Annotation         | Description                                      |
|--------------------|--------------------------------------------------|
| `@Override`        | Vérifie qu'une méthode redéfinit bien une méthode héritée |
| `@Deprecated`      | Indique qu'un élément ne doit plus être utilisé |
| `@SuppressWarnings`| Supprime les avertissements du compilateur      |
| `@FunctionalInterface` | Vérifie qu'une interface respecte le contrat d'une interface fonctionnelle |
| `@NotNull`, `@Nullable` | Indiquent les contraintes de nullité (souvent utilisées avec des outils comme IntelliJ ou Lombok) |
| `@Entity`, `@Table`, `@Id` | Utilisées en JPA pour la persistance des données |
| `@Autowired`, `@Component`, `@Service` | Utilisées dans Spring pour l'injection de dépendances |



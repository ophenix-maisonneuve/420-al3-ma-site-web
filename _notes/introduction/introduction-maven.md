---
layout: default
title: "Introduction à Maven"
parent: "Introduction à Java et ses outils"
nav_order: 2
---

# Introduction à Maven

Apache Maven est un outil de gestion de projet et d'automatisation de la construction pour les projets Java. Il permet de compiler, tester, empaqueter et déployer des applications de manière standardisée.

---

## Cycle de vie de Maven

Maven fonctionne selon un **cycle de vie** composé de plusieurs **phases**, dont les principales sont :

1. **validate** : vérifie que le projet est correct.
2. **compile** : compile le code source.
3. **test** : exécute les tests unitaires.
4. **package** : crée un fichier JAR ou WAR.
5. **verify** : vérifie que l'artefact généré est valide.
6. **install** : installe l'artefact généré dans le dépôt local.
7. **deploy** : déploie l'artefact dans un dépôt distant.

![Cycle de vie Maven](../assets/images/maven-lifecycle.png)

---

## Structure d'un projet Maven
Maven fonctionne sur le principe de convention plutôt que configuration (*convention over configuration*), c'est-à-dire qu'il assume certaines conventions qui, si elles sont respectées, facilitent grandement l'utilisation. Par convention, la structure d'un projet Maven est la suivante:
```
mon-projet/
├── pom.xml                  # Fichier de configuration principal du projet Maven
├── src/
│   ├── main/
│   │   ├── java/            # Code source Java principal
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── App.java
│   │   └── resources/       # Fichiers de configuration et ressources (ex: application.properties)
│   └── test/
│       ├── java/            # Code source des tests unitaires
│       │   └── com/
│       │       └── example/
│       │           └── AppTest.java
│       └── resources/       # Ressources pour les tests
├── target/                  # Répertoire généré contenant les fichiers compilés
└── README.md                # Documentation du projet (optionnel)
        
```

### Création à l'aide d'archétypes (*archetype*)
Les archétypes (*archetypes*) Maven sont des modèles de projets prédéfinis. Ils sont très utiles au démarrage d'un projet, car ils permettent d'automatiser la création d'une structure de projet au lieu d'avoir à tout créer manuellement. Pour un projet simple, la commande suivante permet de générer une structure de base:

```bash
mvn archetype:generate [-D archetypeArtifactId=maven-archetype-quickstart]
```

{: .highlight}
> Le paramètre `-D archetypeArtifactId` sert à spécifier l'archétype à utiliser. Il est optionnel; s'il est omis, Maven présentera une (très longue...) liste d'archétypes utilisables. 

---

## Structure du fichier `pom.xml`

Le fichier `pom.xml` est le cœur du projet Maven. Voici les sections importantes :

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.exemple</groupId>
    <artifactId>mon-projet</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <!-- Dépendances ici -->
    </dependencies>

    <build>
        <plugins>
            <!-- Plugins ici -->
        </plugins>
    </build>
</project>
```

---

## Plugins Maven

Les plugins Maven sont des modules permettent d'étendre les fonctionnalités du cycle de vie de construction. Ils sont utilisés pour compiler du code, exécuter des tests, créer des fichiers JAR, déployer des artefacts, etc. On peut rattacher l'exécution d'un plugin à l'une des phases du cycle de vie de Maven, ce qui permet d'ajouter des fonctionnalités et de personnaliser la construction.

## Exemple 1 : Créer et exécuter un fichier `.jar`

### Structure du projet :
```
mon-projet/
├── pom.xml
└── src/
    └── main/
        └── java/
            └── com/
                └── exemple/
                    └── App.java
```

Structure minimale du `pom.xml` :

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.exemple</groupId>
  <artifactId>mon-projet</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.4.2</version>
        <configuration>
          <archive>
            <manifest>
              <mainClass>com.exemple.Main</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Classe Java :

```java
package com.exemple;

public class Main {
    public static void main(String[] args) {
        System.out.println("Bonjour Maven !");
    }
}
```

Compilation :
```bash
mvn package
```
Le fichier `.jar` sera généré dans le dossier `target/mon-projet-1.0-SNAPSHOT.jar`.

Exécution :
```bash
java -jar target/mon-projet-1.0-SNAPSHOT.jar
```

---

## Exemple 2 : Exécuter le programme avec `exec:java`

### Ajouter le plugin dans `pom.xml` :
```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>3.6.2</version>
      <configuration>
        <mainClass>com.exemple.App</mainClass>
      </configuration>
    </plugin>
  </plugins>
</build>
```

### Commande pour exécuter :
```bash
mvn compile
mvn exec:java
```

Cela exécutera la méthode `main()` de la classe `App`.

---

## Références utiles

- [Documentation officielle de Maven](https://maven.apache.org/guides/index.html)
- [Liste complète du cycle de vie de Maven](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)

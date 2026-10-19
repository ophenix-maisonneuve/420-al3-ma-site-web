---
layout: default
title: "Introduction à Java"
parent: "Introduction à Java et ses outils"
nav_order: 1
---

# Introduction à Java

Java est un langage de programmation orienté objet, robuste et largement utilisé dans le développement d'applications, allant des applications mobiles aux systèmes embarqués et aux applications d'entreprise.

## Typage en Java

Java est un langage **fortement typé** et **statiquement typé**.

### Typage fort
Un langage est **fortement typé** lorsqu’il n’autorise pas les conversions implicites dangereuses entre types. En Java, chaque variable a un type bien défini, et les conversions doivent être explicites.

```java
int x = 5;
String s = "10";
int y = x + Integer.parseInt(s); // conversion explicite
```

En revanche, un langage **faiblement typé** comme JavaScript permet des conversions implicites :

```javascript
console.log("5" + 3); // "53" — concaténation implicite
console.log("5" - 3); // 2 — conversion implicite en nombre
```

### Typage statique
Un langage est **statiquement typé** lorsque les types sont connus et vérifiés à la compilation. En Java, cela signifie que le compilateur vérifie que les types sont compatibles avant d’exécuter le programme.

```java
int age = 25;
age = "trente"; // ❌ Erreur de compilation
```

En comparaison, un langage **dynamiquement typé** comme Python détermine les types à l’exécution :

```python
age = 25       # type int
age = "trente" # devient une chaîne — autorisé
```

### Résumé

| Langage      | Typage statique/dynamique | Typage fort/faible |
|--------------|----------------------------|---------------------|
| Java         | Statique                   | Fort                |
| Python       | Dynamique                  | Plutôt fort         |
| JavaScript   | Dynamique                  | Faible              |


## *Java Virtual Machine* (JVM) et bytecode

Java ne compile pas directement en code machine. Au lieu de cela, le compilateur Java (`javac`) transforme le code source `.java` en **bytecode** (`.class`), un format intermédiaire.

Ce bytecode est ensuite exécuté par la **Java Virtual Machine (JVM)**, qui est une machine virtuelle capable d'exécuter du bytecode sur n'importe quelle plateforme (Windows, macOS, Linux, etc.).

C'est ce qui rend Java **portable** : "écris une fois, exécute partout" (*write once, run anywhere*).

## Le compilateur JIT (*Just-In-Time*)

La JVM utilise un **compilateur JIT** pour améliorer les performances. Le JIT compile à la volée les portions de bytecode les plus utilisées en **code machine natif**, ce qui accélère l'exécution.

Cela permet à Java d'avoir des performances proches de celles des langages compilés comme C++, tout en conservant la flexibilité d'une machine virtuelle.


## Références utiles

- [Documentation officielle de Java](https://docs.oracle.com/en/java/)
- [Learn Java](https://dev.java/learn/)


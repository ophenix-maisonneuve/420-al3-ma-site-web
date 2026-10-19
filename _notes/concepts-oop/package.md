---
layout: default
title: "Package"
parent: "Concepts de programmation orientée objet"
nav_order: 1
---

# Package

En Java, un **package** est un mécanisme utilisé pour organiser les classes et les interfaces dans des groupes logiques. Cela permet de :

- Éviter les conflits de noms.
- Faciliter la maintenance du code.
- Contrôler l'accès avec les modificateurs (`public`, `protected`, etc.).

## Déclaration d'un package

```java
package com.monentreprise.monmodule;

public class MaClasse {
    // Code ici
}
```

## Structure des fichiers
Un package correspond à un **dossier** dans le système de fichiers. Par exemple :

```
src/
└── com/
    └── monentreprise/
        └── monmodule/
            └── MaClasse.java
```

## Importation de classes

```java
import com.monentreprise.monmodule.MaClasse;
```

---

<details markdown="1">

<summary markdown="span"><strong>Comparaison avec les packages en Python</strong></summary>


### Packages en Python

En Python, un package est un **dossier contenant un fichier `__init__.py`** (même vide). Il permet d’organiser les modules (fichiers `.py`) :

```python
# Structure
mon_package/
├── __init__.py
├── module1.py
└── module2.py
```

### Importation

```python
from mon_package import module1
```

### Différences clés

| Aspect               | Java                          | Python                        |
|----------------------|-------------------------------|-------------------------------|
| Structure            | Basée sur les dossiers        | Dossiers + `__init__.py`      |
| Compilation          | Oui (bytecode `.class`)       | Non (interprété)              |
| Contrôle d'accès     | Modificateurs (`public`, etc.)| Convention (`_privé`)         |
| Nom de package       | Doit correspondre au dossier  | Plus flexible                 |

</details>
---
layout: default
title: "Classe abstraite"
parent: "Concepts de programmation orientée objet"
nav_order: 8
---

# Classe abstraite

## Définition
Une **classe abstraite** peut contenir des méthodes implémentées et des méthodes abstraites. Elle sert de base pour d'autres classes. Si une **classe abstraite** implémente une **interface**, elle n'a pas à implémenter toutes les méthodes de l'interface. Les méthodes non-implémentées devront être implémentées par les **sous-classes**.

## Exemple en Java
```java
public abstract class AbstractVehicule implements Vehicule {
    private final String marque;

    public AbstractVehicule(String marque) {
        this.marque = marque;
    }

    // Implémentation par défaut
    // L'absence du mot-clé final permet la redéfinition dans les sous-classes
    public void arreter() {
        System.out.println(marque + " s'arrête.");
    }

    // Implémentation finale.
    // Le mot-clé final empêche la redéfinition dans les sous-classes
    public final void demarrer() {
        System.out.println(marque + " démarre " + getQualificatif());
    }

    // Méthode abstraite (mot-clé abstract)
    // On force les sous-classes à fournir l'implémentation
    protected abstract String getQualificatif();
}
```

```java
public class Voiture extends AbstractVehicule {

    public Voiture(String marque) {
        super(marque);
    }

    @Override
    protected String getQualificatif() {
        return "avec un vrombissement!";
    }
}
```

```java
public class Camion extends AbstractVehicule {

    public Camion(String marque) {
        super(marque);
    }

    @Override
    protected String getQualificatif() {
        return "dans un nuage de fumée noire!";
    }
}
```

<details markdown="1">
<summary markdown="span"><strong>Équivalent Python</strong></summary>

```python
from abc import ABC, abstractmethod

class AbstractVehicule(ABC):
    def __init__(self, marque):
        self._marque = marque

    # Implémentation par défaut
    # En Python, il n'y a pas de mot-clé 'final' natif, mais on peut utiliser des conventions ou des outils comme @final
    def arreter(self):
        print(f"{self._marque} s'arrête.")

    # Implémentation finale (avec @final depuis Python 3.8)
    from typing import final

    @final
    def demarrer(self):
        print(f"{self._marque} démarre {self.get_qualificatif()}")

    # Méthode abstraite
    @abstractmethod
    def get_qualificatif(self):
        pass
```

</details>

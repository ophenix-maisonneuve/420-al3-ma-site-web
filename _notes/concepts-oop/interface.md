---
layout: default
title: "Interface"
parent: "Concepts de programmation orientée objet"
nav_order: 7
---

# Interface

## Définition
Une **interface** définit un contrat que les classes doivent respecter. Elle ne contient normalement que des signatures de méthodes *public*.

## Exemple en Java
```java
public interface Vehicule {
    void demarrer();
    void arreter();
}
```

<details markdown="1">
<summary markdown="span"><strong>Équivalent Python</strong></summary>

```python
from abc import ABC, abstractmethod

class Vehicule(ABC):
    @abstractmethod
    def demarrer(self):
        pass

    @abstractmethod
    def arreter(self):
        pass
```

</details>

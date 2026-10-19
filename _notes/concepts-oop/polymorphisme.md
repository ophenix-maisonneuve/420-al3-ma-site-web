---
layout: default
title: "Polymorphisme"
parent: "Concepts de programmation orientée objet"
nav_order: 6
---

# Polymorphisme

En programmation orientée objet (POO), le polymorphisme est un principe permettant à des objets de réagir **différemment** à une **même méthode**, selon leur **type** spécifique. Le mot vient du grec polus (plusieurs) et morphe (forme), ce qui signifie littéralement « plusieurs formes ».

Il existe deux formes de polymorphisme en Java :

- **Polymorphisme statique** (*surcharge de méthode*)
- **Polymorphisme dynamique** (*redéfinition de méthode*)

---

## Polymorphisme statique

```java
public class Vehicule {
    public void demarrer() {
        System.out.println("Le véhicule démarre.");
    }

    // Polymorphisme statique : surcharge de méthode
    public void demarrer(String message) {
        System.out.println("Le véhicule démarre avec le message : " + message);
    }
}
```

### Explication
- La méthode `demarrer()` est **surchargée** : deux méthodes avec le même nom mais des **paramètres différents**.
- Le choix de la méthode se fait **à la compilation** selon les arguments fournis, d'où le terme **statique**.

---

## Polymorphisme dynamique

```java
public class Voiture extends Vehicule {
    // Polymorphisme dynamique : redéfinition de méthode
    @Override
    public void demarrer() {
        System.out.println("La voiture démarre avec un vrombissement.");
    }
}
```

### Explication
- La méthode `demarrer()` est **redéfinie** dans la classe `Voiture`.
- Le choix de la méthode se fait **à l'exécution**, selon le type réel de l'objet, d'où le terme **dynamique**.

---

## Exemple d'utilisation

```java
public class Main {
    public static void main(String[] args) {
        Vehicule v1 = new Vehicule();
        v1.demarrer(); // Le véhicule démarre.
        v1.demarrer("Prêt à partir !"); // Le véhicule démarre avec le message : Prêt à partir !

        Vehicule v2 = new Voiture();
        v2.demarrer(); // La voiture démarre avec un vrombissement.
    }
}
```

---

<details markdown="1">
<summary markdown="span"><strong>Équivalent Python</strong></summary>

```python
class Vehicule:
    def demarrer(self, message=None):
        if message:
            print(f"Le véhicule démarre avec le message : {message}")
        else:
            print("Le véhicule démarre.")

class Voiture(Vehicule):
    def demarrer(self, message=None):
        print("La voiture démarre avec un vrombissement.")

# Exemple d'utilisation
v1 = Vehicule()
v1.demarrer()
v1.demarrer("Prêt à partir !")

v2 = Voiture()
v2.demarrer()
```

</details>

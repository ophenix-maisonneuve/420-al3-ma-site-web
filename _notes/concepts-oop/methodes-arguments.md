---
layout: default
title: "Méthodes et arguments"
parent: "Concepts de programmation orientée objet"
nav_order: 4
---

# Méthodes et arguments

Java passe toujours les arguments **par valeur** à une méthode. Lorsque l'on passe un type primitif (int, double, boolean, etc), on passe **une copie de la valeur primitive**. Lorsque l'on passe un objet, on passe **copie de sa référence**. Cela signifie que lorsqu’on transmet un objet à une méthode, **on ne transmet pas l’objet lui-même**, mais **une copie de son adresse en mémoire**.

## Exemple :

```java
public class Vehicule {
    private String marque;

    public Vehicule(String marque) {
        this.marque = marque;
    }

    public void setMarque(String marque) {
        this.marque = marque;
    }

    public void afficher() {
        System.out.println("Marque : " + marque);
    }

    public static void changerVoiture(Vehicule v) {
        v.setMarque("Tesla"); // Modifie l'objet pointé
        v = new Vehicule("BMW"); // Ne modifie pas la voiture originale
    }

    public static void main(String[] args) {
        Vehicule maVoiture = new Vehicule("Toyota");
        changerVoiture(maVoiture);
        maVoiture.afficher(); // Affiche : Marque : Tesla
    }
}
```

## Explication

- La méthode `modifierVehicule` reçoit une **copie de la référence** vers l’objet `v1`.
- Elle peut **modifier l’état interne** de l’objet (ici, la marque), car elle pointe vers le **même objet en mémoire**.
- Le changement vers BMW n’a aucun effet sur maVoiture, car la nouvelle instance est assignée à une copie de la référence, pas à l’original.

{:.highlight}
> Java **ne passe pas les objets par référence** comme en C++. Il passe **la référence par valeur**. Cela peut être déroutant, mais cela signifie :
>
> - Les **modifications internes** à l’objet sont visibles à l’extérieur.
> - Mais si on **réassigne** la référence dans la méthode, cela **n’affecte pas** la variable d’origine.

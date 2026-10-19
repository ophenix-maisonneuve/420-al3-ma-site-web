---
layout: default
title: "Accesseurs"
parent: "Concepts de programmation orientée objet"
nav_order: 3
---
# Accesseurs

En Java, les **accesseurs** sont des méthodes utilisées pour **lire** ou **modifier** les valeurs des champs privés d'une classe. On les appelle communément **getters** et **setters**.

---

## Exemple

```java
public class Vehicule {
    private String marque;
    private int annee;

    // Constructeur
    public Vehicule(String marque, int annee) {
        this.marque = marque;
        this.annee = annee;
    }

    // Getter pour marque
    public String getMarque() {
        return marque;
    }

    // Setter pour marque
    public void setMarque(String marque) {
        this.marque = marque;
    }

    // Getter pour annee
    public int getAnnee() {
        return annee;
    }

    // Setter pour annee
    public void setAnnee(int annee) {
        this.annee = annee;
    }

    public void afficherInfos() {
        System.out.println("Marque : " + marque + ", Année : " + annee);
    }
    
    public static void main(String[] args) {
        Vehicule v = new Vehicule("Toyota", 2020);
        v.afficherInfos();

        // Modification via setters
        v.setMarque("Honda");
        v.setAnnee(2022);

        // Lecture via getters
        System.out.println("Nouvelle marque : " + v.getMarque());
        System.out.println("Nouvelle année : " + v.getAnnee());
    }
}
```

---

## Pourquoi utiliser des accesseurs ?

- Pour **protéger les données** internes d'une classe (encapsulation)
- Pour **contrôler l'accès** et la modification des champs
- Pour permettre des **vérifications ou transformations** lors de la lecture ou l'écriture

{: .highlight}
> À titre comparatif, Python privilégie la simplicité; il n'est donc pas pratique courante d'écrire explicitement des accesseurs comme en Java. Cependant, il est possible d'utiliser le décorateur `@property` si l'on désire un contrôle plus fin sur la logique d'accès à une propriété.

<details markdown="1">

<summary markdown="span"><strong>Voir un exemple</strong></summary>

```python
class Vehicule:
    def __init__(self, marque):
        self._marque = marque

    @property
    def marque(self):
        return self._marque

    @marque.setter
    def marque(self, valeur):
        if valeur:
            self._marque = valeur

v = Vehicule("Toyota")
print(v.marque)  # Accès via le getter
v.marque = "Honda"  # Utilise le setter
print(v.marque)
```
</details>
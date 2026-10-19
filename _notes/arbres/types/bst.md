---
layout: default
title: "Arbre binaire de recherche"
parent: "Arbres"
nav_order: 1
published: true
---

# Arbre binaire de recherche (BST)

## Structure

Un **arbre binaire de recherche (*binary search tree - BST*)** est un arbre binaire dans lequel chaque noeud possède au plus deux enfants : un enfant gauche et un enfant droit. Les valeurs dans l'arbre sont organisées de manière à ce que pour chaque noeud :

- Les valeurs dans le sous-arbre gauche sont inférieures à la valeur du noeud.
- Les valeurs dans le sous-arbre droit sont supérieures à la valeur du noeud.

![Illustration BST](https://upload.wikimedia.org/wikipedia/commons/d/da/Binary_search_tree.svg)

{: .highlight}
> Si l'arbre accepte les doublons, les valeurs égales peuvent être placées dans le sous-arbre de gauche ou de droite. L'important est de demeurer constant (toujours à droite ou toujours à gauche). Pour les exemples sur cette page, nous assumerons toujours que les doublons sont insérés dans le sous-arbre de **gauche**.

```java
class Node {
    private int data;
    private Node left;
    private Node right;

    public Node(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }

    public Node getLeft() {
        return left;
    }

    public Node getRight() {
        return right;
    }

    public void setData(int data) {
        this.data = data;
    }

    public void setLeft(Node left) {
        this.left = left;
    }

    public void setRight(Node right) {
        this.right = right;
    }
}

class BinarySearchTree {
    
    // Noeud situé à la racine de l'arbre
    private Node root;

    // les méthodes pour les opérations seront ajoutées ici, voir prochaine sections...
    // ...
}

```

## Opérations principales

### Insertion (ajout)
La complexité moyenne pour l'insertion est **O(log n)** si l'arbre est équilibré, mais peut atteindre **O(n)** dans le pire cas (arbre dégénéré).
<details markdown="1">
<summary markdown="span">Insertion dans un BST</summary>

```java
public Node insert(Node root, int value) {
    // Cas de base : on crée un nouveau noeud avec la valeur et on le retourne
    if (root == null) {
        return new Node(value);
    } 
    if (value <= root.getValue()) {
        root.setLeft(insert(root.getLeft(), value));
    } else if (value > root.getValue()) {
        root.setRight(insert(root.getRight(), value));
    } 
    return root;
}
```
</details>

### Suppression
La suppression a une complexité moyenne de **O(log n)**, mais peut être **O(n)** si l'arbre est très déséquilibré. Elle nécessite parfois un rééquilibrage. La suppression fonctionne selon la logique suivante:
- Si le noeud à supprimer n'a aucun enfant (feuille), on supprime simplement la feuille. Du point de vue du parent, la feuille est remplacée par `null`.
- Si le noeud à supprimer a un seul enfant, on remplace ce noeud par son enfant. Du point de vue du parent, sous petit-enfant devient son enfant direct.
- Si le noeud à supprimer a deux enfants:
   - On trouve le **successeur** du noeud à supprimer (la prochaine valeur en ordre croissant, ce qui équivaut à la plus petite valeur du sous-arbre de droite).
   - On remplace la **valeur** du noeud à supprimer par la **valeur** du successeur (autrement dit, le noeud n'est pas vraiment supprimé, mais sa valeur change).
   - On supprime le noeud du successeur dans le sous-arbre de droite.

<details markdown="1">
<summary markdown="span">Suppression dans un BST</summary>

```java
public Node delete(Node root, int value) {
    // Cas de base : sous-arbre vide : on retourne un noeud vide
    if (root == null) {
        return null;
    }

    // Parcours récursif : on cherche le noeud à supprimer
    if (value < root.getValue()) {
        // La valeur est plus petite : descendre à gauche
        root.setLeft(delete(root.getLeft(), value));
    } else if (value > root.getValue()) {
        // La valeur est plus grande : descendre à droite
        root.setRight(delete(root.getRight(), value));
    } else {
        // Noeud trouvé : appliquer la logique de suppression

        // Cas 0 ou 1 enfant : remplacer par l'enfant
        if (root.getLeft() == null) {
            return root.getRight();
        }
        if (root.getRight() == null) {
            return root.getLeft();
        }

        // Cas 2 enfants : remplacer la valeur par le successeur (minimum du sous-arbre droit)
        Node minNode = findMin(root.getRight());
        root.setValue(minNode.getValue());

        // Supprimer le successeur dans le sous-arbre droit
        root.setRight(delete(root.getRight(), minNode.getValue()));
    }

    // Retourner la racine mise à jour
    return root;
}

// Méthode utilitaire pour trouver la plus petite valeur dans un sous-arbre (le successeur)
private Node findMin(Node node) {
    while (node.getLeft() != null) {
        node = node.getLeft();
    }
    return node;
}
```
</details>

### Recherche
La recherche dans un BST est en moyenne **O(log n)**, mais peut être **O(n)** si l'arbre est très déséquilibré.
<details markdown="1">
<summary markdown="span">Recherche dans un BST</summary>

```java
public boolean search(Node root, int value) {
    // Cas trivial : arbre vide
    if (root == null) {
        return false;
    }

    // Cas de base : la valeur est trouvée
    if (value == root.getValue()) {
        return true;
    }

    // Recherche récursive : si la valeur est plus petite que la racine, on va à gauche, sinon à droite
    if (value <= root.getValue()) {
        return search(root.getLeft(), value);
    } else {
        return search(root.getRight(), value);
    }
}
```
</details>

### Parcours
Le parcours (infixe, préfixe, suffixe) visite tous les noeuds, donc la complexité est **O(n)**.

| Parcours    | Ordre des visites         | Usage principal                     |
|-------------|---------------------------|-------------------------------------|
| Infixe      | Gauche → Racine → Droite | Affichage trié (ordre croissant)   |
| Préfixe     | Racine → Gauche → Droite | Reconstruction, sérialisation      |
| Suffixe     | Gauche → Droite → Racine | Suppression, libération mémoire    |

<details markdown="1">
<summary markdown="span">Parcours infixe</summary>

Le parcours **infixe** (*in-order*) parcourt l'arbre dans son ordre naturel. Il visite dans l'ordre le sous-arbre de gauche, la racine, puis le sous-arbre de droite (ou l'inverse si on veut un ordre décroissant). Il récupère et affiche donc les valeurs triées en ordre croissant (gauche, racine, droite) ou décroissant (droite, racine, gauche).
```java
public void inorder(Node root) {
    if (root != null) {
        inorder(root.getLeft());
        System.out.print(root.getValue() + " ");
        inorder(root.getRight());
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">Parcours préfixe</summary>

Le parcours **préfixe** récupère d'abord la racine, puis visite le sous-arbre de gauche et celui de droite. Ce type de parcours est généralement utile pour afficher l'arbre sous forme de texte ou le reconstruire à partir de sa forme sérialisée : on construit tous les noeuds de haut en bas.
```java
public void preorder(Node root) {
    if (root != null) {
        System.out.print(root.getValue() + " ");
        preorder(root.getLeft());
        preorder(root.getRight());
    }
}
```
</details>
<details markdown="1">
<summary markdown="span">Parcours suffixe</summary>

Le parcours **suffixe** récupère d'abord le sous-arbre de gauche, puis le sous-arbre de droite, et finalement la racine. Ce type de parcours est généralement utile pour des opérations de suppression de sous-arbres complets où l'on veut supprimer les feuilles avant les noeuds, en remontant ainsi jusqu'à la racine.
```java
public void postorder(Node root) {
    if (root != null) {
        postorder(root.getLeft());
        postorder(root.getRight());
        System.out.print(root.getValue() + " ");
    }
}
```
</details>

---

## Complexité des opérations

| Opération   | Complexité moyenne | Complexité pire cas |
|-------------|---------------------|----------------------|
| Insertion   | O(log n)            | O(n)                 |
| Recherche   | O(log n)            | O(n)                 |
| Suppression | O(log n)            | O(n)                 |
| Parcours    | O(n)                | O(n)                 |

{: .highlight}
> La complexité dépend de l'équilibre de l'arbre. Un arbre parfaitement équilibré permet des opérations en O(log n), tandis qu'un arbre dégénéré (ressemblant à une liste) entraîne des performances en O(n). Il existe des types d'arbres binaires qui s'assurent que l'arbre demeure bien équilibré, garantissant toujours une complexité **(log n)** pour l'insertion, la suppression et la recherche.

---

## Applications

- Indexation en base de données
- Moteurs de recherche
- Dictionnaires

---

## Liens utiles

- [Vidéo explicative sur BST](https://www.youtube.com/watch?v=mtvbVLK5xDQ)
- [BST interactif](https://www.cs.usfca.edu/~galles/visualization/BST.html)

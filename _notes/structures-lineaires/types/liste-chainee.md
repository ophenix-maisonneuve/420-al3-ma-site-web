---
layout: default
parent: "Structures linéaires"
title: "Liste chaînée"
nav_order: 2
published: true
---

# Liste chaînée

## Structure

### Simple

Une liste chaînée (aussi appelée liste chaînée simple) est composée de noeuds où chaque noeud pointe vers le suivant. Elle permet des insertions et suppressions rapides, surtout en début et en fin de liste (si on garde une référence au dernier noeud).

![Liste chaînée simple](../assets/images/singly-linked-list.webp)
*Image tirée de geeksforgeeks.org*

```java
class Node {
    private int data;
    private Node next;

    public Node(int d) {
        this.data = d;
    }

    public int getData() {
        return data;
    }

    public void setData(int data) {
        this.data = data;
    }

    public Node getNext() {
        return next;
    }

    public void setNext(Node next) {
        this.next = next;
    }
}

class LinkedList {
    private Node head;

    // les méthodes pour les opérations seront ajoutées ici, voir prochaine sections...
    // ...
}
```

### Double

Une liste doublement chaînée(ou liste chaînée double) permet de naviguer dans les deux sens grâce à des références vers le noeud précédent et suivant. En d'autres mots, elle permet aussi d'itérer à reculons dans la liste.

![Liste chaînée double](../assets/images/doubly-linked-list.webp)
*Image tirée de geeksforgeeks.org*

```java
private class Node {
    private int data;
    private Node prev;
    private Node next;

    public Node(int d) {
        this.data = d;
    }

    public int getData() {
        return data;
    }

    public void setData(int data) {
        this.data = data;
    }

    public Node getPrev() {
        return prev;
    }

    public void setPrev(Node prev) {
        this.prev = prev;
    }

    public Node getNext() {
        return next;
    }

    public void setNext(Node next) {
        this.next = next;
    }
}

public class DoublyLinkedList {
    private Node head;

    // les méthodes pour les opérations seront ajoutées ici, voir prochaine section...
    // ...
}
```

## Opérations sur les listes chaînées

### Ajout
Dans une liste chaînée ou doublement chaînée classique qui ne maintient que la référence vers le premier noeud de la liste (*head*), l'insertion en tête de liste est optimale (**O(1)**), car il suffit de remplacer la tête par le nouveau noeud et d'ajuster ses références. Pour tous les autres cas, on considère l'ajout **O(n)**, car au pire cas, on devra itérer sur toute la liste. Cependant, si la liste maintient également une référence vers le dernier noeud de la liste (*tail*), l'insertion en fin de liste sera également **O(1)**.

<details markdown="1">
<summary markdown="span">**Ajout dans une liste chaînée**</summary>

On crée un nouveau noeud et on ajuste les pointeurs.
```java
// Ajoute à la position demandée, 0 = début de liste
public void add(int position, int data) {
    Node newNode = new Node(data);

    // Cas optimal : insertion en tête
    if (position == 0) {
        newNode.setNext(head);
        head = newNode;
        return;
    }

    // Parcours jusqu'au noeud précédent la position
    Node current = head;
    for (int i = 0; i < position - 1 && current != null; i++) {
        current = current.getNext();
    }

    if (current == null) {
        throw new IndexOutOfBoundsException("Position invalide");
    }

    // Insertion
    newNode.setNext(current.getNext());
    current.setNext(newNode);
}


// Ajoute à la fin de la liste
public void add(int data) {
    Node newNode = new Node(data);
    if (head == null) {
        head = newNode;
        return;
    }
    Node current = head;
    while (current.getNext() != null) {
        current = current.getNext();
    }
    current.setNext(newNode);
}
```
</details>

<details markdown="1">
<summary markdown="span">**Ajout dans une liste doublement chaînée**</summary>
Permet d'insérer dans les deux directions.

```java
// Ajoute à la position demandée, 0 = début de la liste
public void add(int position, int data) {
    Node newNode = new Node(data);

    // Cas optimal : insertion en tête
    if (position == 0) {
        newNode.setNext(head);
        if (head != null) {
            head.setPrev(newNode);
        }
        head = newNode;
        return;
    }

    Node current = head;
    for (int i = 0; i < position - 1 && current != null; i++) {
        current = current.getNext();
    }

    if (current == null) {
        throw new IndexOutOfBoundsException("Position invalide");
    }

    newNode.setNext(current.getNext());
    newNode.setPrev(current);
    if (current.getNext() != null) {
        current.getNext().setPrev(newNode);
    }
    current.setNext(newNode);
}

// Ajoute à la fin
public void add(int data) {
    Node newNode = new Node(data);
    if (head == null) {
        head = newNode;
        return;
    }
    Node current = head;
    while (current.getNext() != null) {
        current = current.getNext();
    }
    current.setNext(newNode);
    newNode.setPrev(current);
}
```
</details>

### Suppression
Dans une liste chaînée ou doublement chaînée classique qui ne maintient que la référence vers le premier noeud de la liste (*head*), la suppression du premier noeud est optimale (**O(1)**), car il suffit de remplacer la tête par le deuxième noeud et, dans le cas d'une liste doublement chaînée, d'ajuster la référence arrière de celui-ci. Pour tous les autres cas, on considère la suppression **O(n)**, car au pire cas, on devra itérer sur toute la liste. Cependant, si la liste maintient également une référence vers le dernier noeud de la liste (*tail*), la suppression du dernier noeud est également **O(1)**.

<details markdown="1">
<summary markdown="span">**Suppression dans une liste chaînée**</summary>

```java
public void remove(int position) {

    if (head == null) {
        throw new IndexOutOfBoundsException("Liste vide");
    }

    // Cas optimal : suppression en tête
    if (position == 0) {
        head = head.getNext();
        return;
    }

    // Itération sur la liste pour trouver l'élément qui précède celui à supprimer
    Node current = head;
    for (int i = 0; i < position - 1 && current != null; i++) {
        current = current.getNext();
    }

    // Vérification si la position est invalide
    if (current == null || current.getNext() == null) {
        throw new IndexOutOfBoundsException("Position invalide");
    }

    // Supprimer le noeud désiré en changeant la référence du précédent vers le suivant.
    // Le noeud à supprimer ne sera donc plus référencé dans la liste.
    current.setNext(current.getNext().getNext());
}

```
</details>

<details markdown="1">
<summary markdown="span">**Suppression dans une liste doublement chaînée**</summary>

```java

public void remove(int position) {
    if (head == null) {
        throw new IndexOutOfBoundsException("Liste vide");
    }

    // Cas optimal : suppression en tête
    if (position == 0) {
        head = head.getNext();
        if (head != null) {
            head.setPrev(null);
        }
        return;
    }

    // Itération sur la liste pour trouver l'élément à supprimer
    Node current = head;
    for (int i = 0; i < position && current != null; i++) {
        current = current.getNext();
    }

    if (current == null) {
        throw new IndexOutOfBoundsException("Position invalide");
    }

    // Supprimer le noeud désiré en procédant de la manière suivante:
    // 1 - Assigner comme "next" au noeud précédent le noeud suivant (on saute le noeud à supprimer)
    // 2 - Assigner comme "prev" au noeud suivant le noeud précédent (on saute le noeud à supprimer)
    // Le noeud à supprimer ne sera donc plus référencé dans la liste.
    Node prevNode = current.getPrev();
    Node nextNode = current.getNext();

    if (prevNode != null) {
        prevNode.setNext(nextNode);
    }
    if (nextNode != null) {
        nextNode.setPrev(prevNode);
    }
}

```
</details>

### Tri
Il existe un très grand nombre d'algorithmes de tri sur les structures linéaires, dont les principaux sont:
- Tri à bulles
- Tri par insertion
- Tri rapide (*quick sort*)
- Tri par fusion (*merge sort*)

Dans le cours, nous étudierons plus en détails le tri à bulles pour sa simplicité (même s'il est moins performant - O(n<sup>2</sup>)) ainsi que le tri par fusion, qui offre une bonne performance constante (**O(n log n)**).

<details markdown="1">
<summary markdown="span">**Tri à bulles**</summary>

Le tri à bulles compare chaque paire d'éléments adjacents et les échange si nécessaire, répétant ce processus jusqu'à ce que la liste soit triée.

Video sur YouTube: [Bubble Sort in 2](https://www.youtube.com/watch?v=xli_FI7CuzA)

```java
public void bubbleSort(Node head) {
    if (head == null) {
        return;
    } 

    Node current;
    Node index;

    // Parcourt toute la liste pour chaque élément
    for (current = head; current != null; current = current.getNext()) {
        for (index = head; index.getNext() != null; index = index.getNext()) {
            if (index.getData() > index.getNext().getData()) {
                // Échange des valeurs
                int temp = index.getData();
                index.setData(index.getNext().getData());
                index.getNext().setData(temp);
            }
        }
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">**Tri par fusion (*Merge Sort*)**</summary>

Le tri par fusion divise la liste en deux moitiés, trie chaque moitié récursivement, puis fusionne les deux listes triées.

Video sur YouTube: [Merge Sort in 3](https://www.youtube.com/watch?v=4VqmGXwpLqc)

```java
public Node mergeSort(Node head) {
    // Cas de base : liste vide ou un seul élément (déjà triée)
    if (head == null || head.getNext() == null) {
         return head;
    }

    // Trouver le milieu de la liste pour la diviser en deux
    Node middle = getMiddle(head);
    Node nextOfMiddle = middle.getNext();

    // Couper la liste en deux sous-listes
    middle.setNext(null);

    // Tri récursif des deux moitiés
    Node left = mergeSort(head);
    Node right = mergeSort(nextOfMiddle);

    // Fusion des deux sous-listes triées
    return sortedMerge(left, right);
}

private Node getMiddle(Node head) {
    if (head == null) {
        return head;
    }

    // Utilisation de la technique slow/fast pour trouver le milieu
    Node slow = head;
    Node fast = head.getNext();

    // Avancer fast de 2 pas et slow de 1 pas
    while (fast != null && fast.getNext() != null) {
        slow = slow.getNext();
        fast = fast.getNext().getNext();
    }

    // slow pointe sur le milieu
    return slow;
}

private Node sortedMerge(Node a, Node b) {
    // Si une des listes est vide, retourner l'autre
    if (a == null) {
         return b;
    }
    if (b == null) {
        return a;
    } 

    Node result;

    // Comparer les têtes des deux listes et choisir la plus petite
    if (a.getData() <= b.getData()) {
        result = a;
        // Fusionner le reste récursivement
        result.setNext(sortedMerge(a.getNext(), b));
    } else {
        result = b;
        result.setNext(sortedMerge(a, b.getNext()));
    }

    return result;
}
```
</details>


### Recherche
Dans une liste chaînée ou doublement chaînée, la recherche binaire n'offre pas d'avantage de performance et n'est donc pas applicable. En effet, la recherche binaire nécessite un accès direct à l’élément du milieu, ce qui est inefficace dans une liste chaînée (accès linéaire), car cela impliquerait de toujours itérer sur la liste jusqu'au centre (donc **O(n)**). La recherche linéaire reste donc la méthode la plus adaptée pour les listes chaînées, malgré sa complexité **O(n)**.

<details markdown="1">
<summary markdown="span">**Recherche linéaire**</summary>
Il s'agit du seul algorithme de recherche valable pour les listes chaînées. Une recherche binaire est théoriquement possible, mais n'offre aucun avantage.

```java
Node current = head;
while (current != null) {
    if (current.getData() == target) {
        return true;
    }
    current = current.getNext();
}
return false;
```

</details>

{: .highlight}
> Java fournit déjà une implémentation de liste chaînée double qui se nomme `LinkedList` (voir plus de détails [ici](../notes/linkedlist))
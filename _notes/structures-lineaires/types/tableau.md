---
layout: default
parent: "Structures linéaires"
title: "Tableau"
nav_order: 1
published: true
---

# Tableau (Array)

Un tableau est une structure de données à taille fixe où chaque élément est accessible directement par son index. Il est efficace pour les accès rapides mais coûteux pour les insertions et suppressions.

```java
public class MonTableau {
    public static void main(String[] args) {
        int[] tableau = new int[5];
        tableau[0] = 10;
        tableau[1] = 20;
        tableau[2] = 30;
        for (int i = 0; i < tableau.length; i++) {
            System.out.println(tableau[i]);
        }
    }
}
```

## Opérations sur les tableaux

### Ajout
Dans un tableau, l'ajout peut se faire de deux façons différentes selon l'objectif visé.
- **Écrasement (*overwrite*)** : le nouvel élément est écrit à l'index spécifié et écrase l'ancienne valeur (si une valeur était présente). Un ajout de ce type est très simple et a toujours une complexité O(1).
- **Élément additionnel** : le nouvel élément est écrit à l'index spécifié et les éléments suivants sont décalés. Un ajout de ce type a une complexité pire cas de O(n), car on doit recopier tous les éléments dans un nouveau tableau (si le tableau initial n'a plus d'espace libre) ou itérer sur tous les éléments suivant l'insertion et les décaler (s'il reste de la place dans le tableau).
<details markdown="1">
<summary markdown="span">**Ajout simple (avec possible écrasement)**</summary>

Voici une insertion simple, où l'on écrase potentiellement l'élément qui était déjà présent à l'index donné.
```java
int[] array = new int[5];
array[0] = 10; // ajout en début
array[2] = 30; // ajout au milieu
array[4] = 50; // ajout en fin
```
</details>

<details markdown="1">
<summary markdown="span">**Ajout avec décalage ou redimensionnement**</summary>

Dans la majorité des cas, on veut plutôt ajouter aux éléments déjà existants, ce qui demande plutôt de décaler les éléments ou même de recréer le tableau avec une taille plus grande. On effectue ainsi toujours une opération **O(n)** : soit on recopie tous les éléments dans un nouveau tableau, soit on décale vers la droite tous les éléments qui suivent l'élément ajouté.

```java
int[] original = {10, 20, 30, 40, 50};
int newValue = 25;
int insertIndex = 2;

// Création d'un nouveau tableau avec une taille augmentée
int[] resized = new int[original.length + 1];

// Copier les éléments avant l'index d'insertion
for (int i = 0; i < insertIndex; i++) {
    resized[i] = original[i];
}

// Insérer la nouvelle valeur
resized[insertIndex] = newValue;

// Décaler les éléments restants
for (int i = insertIndex; i < original.length; i++) {
    resized[i + 1] = original[i];
}
```

{: .highlight}
> Pour éviter de recréer un tableau à chaque insertion, certaines structures (comme **ArrayList** en Java) anticipent les ajouts en allouant un espace supplémentaire lors du redimensionnement. Plutôt que d’augmenter la taille d’une seule case à la fois, elles créent un tableau plus grand — souvent 1,5 à 2 fois la taille actuelle — afin de réduire le nombre de copies coûteuses. Cette stratégie amortit le coût des insertions : la plupart des ajouts se font en temps constant, et seules les opérations de redimensionnement impliquent une copie complète.
</details>

### Suppression
Dans un tableau, la suppression peut se faire de deux façons différentes selon l'objectif visé.
- **Décalage** : les éléments à la droite de l'élément supprimé sont décalés d'une case vers la gauche. Cela laisse une case vide à la fin du tableau, qui pourra être utilisée en cas d'ajout. Au pire cas, cette opération est linéaire (**O(n)**).
- **Redimensionnement** : un nouveau tableau de taille **n-1** est créé, et tous les éléments sauf celui à supprimer y sont recopiés. Cette opération est toujours **O(n)**, puis qu'on doit copier tous les éléments un à la fois.
<details markdown="1">
<summary markdown="span">**Suppression simple (sans redimensionnement)**</summary>

```java
int[] array = {10, 20, 30, 40};
// Suppression au milieu (index 1)
for (int i = 1; i < array.length - 1; i++) {
    array[i] = array[i + 1];
}
```
</details>

<details markdown="1">
<summary markdown="span">**Suppression avec redimensionnement**</summary>

```java
int[] original = {10, 20, 30, 40};
int newValue = 25;
int deleteIndex = 3;

// Création d'un nouveau tableau avec une taille réduite
int[] resized = new int[original.length - 1];

// Copier les éléments avant l'index à supprimer
for (int i = 0; i < deleteIndex; i++) {
    resized[i] = original[i];
}

// Décaler les éléments restants
for (int i = deleteIndex + 1; i < original.length; i++) {
    resized[i - 1] = original[i];
}
```
</details>


### Tri

Il existe un très grand nombre d'algorithmes de tri sur les tableaux, dont les principaux sont:
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
public void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
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
public void mergeSort(int[] array, int left, int right) {
    if (left < right) {
        int middle = (left + right) / 2;

        // Tri récursif des deux moitiés de tableau
        mergeSort(array, left, middle);
        mergeSort(array, middle + 1, right);

        // Fusion des moitiés triées
        merge(array, left, middle, right);
    }
}

private void merge(int[] array, int left, int middle, int right) {
    int leftSize = middle - left + 1;
    int rightSize = right - middle;

    int[] leftArray = new int[leftSize];
    int[] rightArray = new int[rightSize];

    // Copie des données dans des tableaux temporaires
    for (int i = 0; i < leftSize; i++) {
        leftArray[i] = array[left + i];
    }
    for (int j = 0; j < rightSize; j++) {
        rightArray[j] = array[middle + 1 + j];
    }

    int i = 0, j = 0;
    int k = left;

    // Fusion des tableaux temporaires
    while (i < leftSize && j < rightSize) {
        if (leftArray[i] <= rightArray[j]) {
            array[k++] = leftArray[i++];
        } else {
            array[k++] = rightArray[j++];
        }
    }

    // Copie des éléments restants du tableau de gauche
    while (i < leftSize) {
        array[k++] = leftArray[i++];
    }

    // Copie des éléments restants du tableau de droite
    while (j < rightSize) {
        array[k++] = rightArray[j++];
    }
}
```
</details>


### Recherche
Comme un tableau permet l'accès direct aux éléments par leur index, la recherche binaire est possible si on trie d'abord le tableau. Il existe donc deux algorithmes possibles pour la recherche:
- **Recherche linéaire** : Les éléments du tableau sont comparés un par un à la valeur recherchée. Dans le pire cas, cette opération a une complexité **O(n)**.
- **Recherche binaire** : À chaque itération, on compare la valeur recherchée avec la valeur médiane du tableau. Si la valeur est inférieure, l'itération suivante se fera sur la moitié de gauche, et si elle est supérieure, l'itération suivante se fera sur la moitié de droite. Le processus est répété jusqu'à ce qu'il ne reste qu'un élément, qui sera égal ou non à la valeur recherchée. Cette opération a une complexité **O(log n)**, car chaque itération divise la quantité de données analysée par deux.
<details markdown="1">
<summary markdown="span">**Recherche linéaire**</summary>
```java
for (int i = 0; i < array.length; i++) {
    if (array[i] == target) {
        return i;
    }
}
```
</details>

<details markdown="1">
<summary markdown="span">**Recherche binaire**</summary>

```java
int left = 0, right = array.length - 1;
while (left <= right) {
    int mid = (left + right) / 2;
    if (array[mid] == target) {
        return mid;
    } else if (array[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
```
</details>

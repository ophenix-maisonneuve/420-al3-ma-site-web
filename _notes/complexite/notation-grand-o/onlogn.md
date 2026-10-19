---
layout: default
title: "Complexité O(n log n) - Linéarithmique"
parent: "Notation Grand O"
nav_order: 1
published: true
---

# Complexité O(n log n) - Linéarithmique

Cette complexité apparaît souvent dans les algorithmes de tri efficaces comme le tri fusion ou le tri rapide. L’algorithme effectue une opération linéaire (parcourir les éléments) combinée à une division répétée (log n). Cela croît plus vite que O(n), mais beaucoup moins que O(n<sup>2</sup>).

## Exemple (tri fusion simplifié)
```java
public class ExempleOnlogn {
    public static void mergeSort(int[] array, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(array, left, mid);
            mergeSort(array, mid + 1, right);
            merge(array, left, mid, right);
        }
    }

    public static void merge(int[] array, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left;
        int j = mid + 1;
        int k = 0;

        while (i <= mid && j <= right) {
            if (array[i] <= array[j]) {
                temp[k++] = array[i++];
            } else {
                temp[k++] = array[j++];
            }
        }
        while (i <= mid) {
            temp[k++] = array[i++];
        } 
        while (j <= right) {
            temp[k++] = array[j++];
        } 
        for (i = left, k = 0; i <= right; i++, k++) {
            array[i] = temp[k];
        } 
    }
}
```

## Allure de la courbe

![Complexité O(n log n)](../assets/images/complexite-onlogn.png)

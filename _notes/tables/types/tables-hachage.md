---
layout: default
parent: "Tables associatives"
title: "Table de hachage"
nav_order: 1
published: true
---

# Tables de hachage
Une **table de hachage** est une **table associative** qui utilise une **fonction de hachage** pour transformer les **clés** en index. Lorsqu'une collision survient (deux clés produisent le même index), plusieurs stratégies sont possibles :

- **Chaînage séparé** : chaque case (*bucket*) contient une liste chaînée des paires clé-valeur qui partagent le même index
- **Sondage linéaire** : on cherche la prochaine case (*bucket*) libre dans le tableau
- **Double hachage** : on applique une seconde fonction de hachage pour calculer le pas de recherche
- D'autres variantes existent (ex. sondage quadratique).

Les tables de hachage sont l'une des deux implémentations les plus fréquentes des tables associatives, l'autre étant les **tables basées sur des arbres** (*tree-based maps*), qui maintiennent les clés triées sans collisions mais avec des opérations en O(log n).

Dans cette page, nous illustrons une implémentation pédagogique basée sur le **chaînage séparé**, nommée **`TableHachage`**, avec :

- **Chaînage séparé** : chaque case (*bucket*) est une **liste simplement chaînée** de paires clé-valeur
- **Facteur de charge** `0,75`
- **Redimensionnement ×2** quand `taille / capacité > 0,75`
- **Clé `null` interdite**, **valeur `null` autorisée**

![Table de hachage avec chaînage séparé](../assets/images/hash-table-chainage-simple.png)


> **Complexités (moyenne)** : `put/get/remove` en **`O(1)` amorti**  
> **Pire cas** : `O(n)` si les collisions se concentrent anormalement.

---

## Structure

```java
// Schéma simplifié utilisé dans tous les extraits ci-dessous.
class Entry {
    private final String key;
    private Object value;
    private Entry next;

    Entry(String key, Object value, Entry next) {
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public String getKey() {
        return key;
    }
    public Object getValue() {
        return value;
    }
    public void setValue(Object value) {
        this.value = value;
    }
    public Entry getNext() {
        return next;
    }
    public void setNext(Entry next) {
        this.next = next;
    }
}

class TableHachage {
    private Entry[] table; // tableau de cases (*buckets*)
    private int size; // nombre de paires (clé, valeur)
    private final float loadFactor; // ex. 0.75f
}

```

---

## Fonctions utilitaires

<details markdown="1">
<summary markdown="span">**indexFor(key)** — transformer le hash en index de case (*bucket*)</summary>

```java
/** Calcule l'index de case (*bucket*) pour une clé donnée. */
private int indexFor(String key) {
    int h = key.hashCode();
    // Masque le bit de signe pour éviter les indices négatifs puis modulo capacité
    return (h & 0x7fffffff) % table.length;
}
```
</details>

<details markdown="1">
<summary markdown="span">**findEntry(key)** — retrouver le paire clé-valeur (si présent)</summary>

```java
/** Retourne la paire clé-valeur (paire clé-valeur) associée à la clé, ou null si absente. */
private Entry findEntry(String key) {
    if (key == null) {
        throw new NullPointerException("La clé ne peut pas être nulle");
    }

    int index = indexFor(key);
    for (Entry e = table[index]; e != null; e = e.getNext()) {
        if (key.equals(e.getKey())) {
            return e;
        }
    }
    return null;
}
```
</details>

---

## Ajout / Mise à jour

<details markdown="1">
<summary markdown="span">**put(key, value)** — insérer ou mettre à jour (O(1) amorti)</summary>

```java
/**
 * Insère la paire (key, value) ou met à jour si la clé existe déjà.
 * @return l'ancienne valeur si mise à jour, sinon null (nouvelle paire clé-valeur).
 */
public Object put(String key, Object value) {
    if (key == null) {
        throw new NullPointerException("La clé ne peut pas être nulle");
    }

    // 1) Redimensionnement éventuel (préventif) pour maintenir le load factor
    if (needsResize()) {
        resize();
    }

    // 2) Index de la case (*bucket*)
    int index = indexFor(key);

    // 3) Parcourir la liste chaînée : si la clé existe → mise à jour
    for (Entry e = table[index]; e != null; e = e.getNext()) {
        if (key.equals(e.getKey())) {
            Object old = e.getValue();
            e.setValue(value);
            return old;
        }
    }

    // 4) Sinon, insertion en tête de liste chaînée (O(1))
    table[index] = new Entry(key, value, table[index]);
    size++;
    return null;
}

/** Vrai si une insertion supplémentaire dépasserait le facteur de charge. */
private boolean needsResize() {
    return size + 1 > (int)(table.length * loadFactor);
}
```
</details>

---

## Suppression

<details markdown="1">
<summary markdown="span">**remove(key)** — retirer une paire (O(1) amorti)</summary>

```java
/**
 * Supprime la paire (key, ?) si présente.
 * @return l'ancienne valeur si supprimée, sinon null.
 */
public Object remove(String key) {
    if (key == null) {
        throw new NullPointerException("La clé ne peut pas être nulle");
    } 

    int index = indexFor(key);
    Entry prev = null;
    Entry curr = table[index];

    while (curr != null) {
        if (key.equals(curr.key)) {
            // On "saute" la paire clé-valeur courante, supprimant ainsi l'entrée
            if (prev == null) {
                table[index] = curr.getNext(); // suppression en tête
            } else {
                prev.setNext(curr.getNext();    // suppression au milieu/fin
            }
            size--;
            return curr.value;
        }
        prev = curr;
        curr = curr.getNext();
    }
    return null; // clé absente
}
```
</details>

---

## Recherche par clé

<details markdown="1">
<summary markdown="span">**get(key)** — retrouver la valeur (O(1) amorti)</summary>

```java
/** Retourne la valeur associée à la clé, ou null si absente. */
public Object get(String key) {
    Entry e = findEntry(key);
    return (e == null) ? null : e.getValue();
}
```
</details>

<details markdown="1">
<summary markdown="span">**containsKey(key) — la clé est‑elle présente ?**</summary>

```java
/** Vrai si la clé est présente, même si la valeur associée est null. */
public boolean containsKey(String key) {
    return findEntry(key) != null;
}
```
</details>

---

## Redimensionnement

<details markdown="1">
<summary markdown="span">**resize()** — doubler la capacité et re‑hacher toutes les paire clé-valeur (O(n))</summary>

```java
/** Double la capacité et re-hache toutes les paire clé-valeur. Coût linéaire, mais rare. */
private void resize() {
    Entry[] oldTable = this.table;
    Entry[] newTable = new Entry[oldTable.length * 2];
    this.table = newTable;

    // Ré-insertion de toutes les paires clé-valeur dans le nouveau tableau de cases (*buckets*)
    for (Entry head : oldTable) {
        Entry e = head;
        while (e != null) {
            Entry next = e.getNext(); // préserver la suite avant de relinker
            int idx = (e.getKey().hashCode() & 0x7fffffff) % newTable.length;
            e.setNext(newTable[idx]);
            newTable[idx] = e;
            e = next;
        }
    }
}
```
</details>

---

## Parcours

<details markdown="1">
<summary markdown="span">**toString()** — parcourir toutes les paire clé-valeur pour afficher (O(n))</summary>

```java
@Override
public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("{");
    boolean first = true;
    for (Entry bucket : table) {
        for (Entry e = bucket; e != null; e = e.getNext()) {
            if (!first) {
                sb.append(", ");
            }
            first = false;
            sb.append(e.getKey()).append("=").append(String.valueOf(e.getValue()));
        }
    }
    sb.append("}");
    return sb.toString();
}
```
</details>

---

## Résumé des complexités

| Opération                      | Moyenne         | Pire cas |
|---                             |---              |---|
| `put(key, value)`              | `O(1)` amorti   | `O(n)` |
| `get(key)` / `containsKey(key)`| `O(1)` amorti   | `O(n)` |
| `remove(key)`                  | `O(1)` amorti   | `O(n)` |
| `resize()` (re‑hachage)        | `O(n)`               | `O(n)` |
| Parcours complet               | `O(n)`          | `O(n)` |

{: .highlight}
> Les performances moyennes reposent sur un **bon hachage** des clés et un **facteur de charge maîtrisé** (ex. `0,75`) qui limite la longueur moyenne des liste chaînées.
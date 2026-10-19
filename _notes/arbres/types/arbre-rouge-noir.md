---
layout: default
parent: "Arbres"
title: "Arbre rouge-noir"
nav_order: 2
published: true
---

# Arbre rouge-noir (*Red-Black Tree*)

## Structure
Un **arbre rouge‑noir** est une spécialisation d'arbre binaire de recherche dans laquelle on a ajouté une propriété de **couleur** et des règles associées qui permettent de préserver l'équilibre de l'arbre. L'arbre rouge-noir garantit donc des opérations en **O(log n)** grâce au **coloriage des noeuds** (rouge/noir) et à des **rotations** qui maintiennent des propriétés d’équilibre. Il respecte les règles du BST (valeurs plus petites à gauche, plus grandes à droite) et ajoute des **propriétés de couleur** pour éviter la dégénérescence.

![Illustration Red-Black Tree](../assets/images/red-black-tree.webp)

*Schéma indicatif d’un arbre rouge‑noir tiré de geeksforgeeks.org*

{: .highlight}
> Si les doublons sont acceptés, il faut rester **constant** (toujours à gauche **ou** toujours à droite). Dans ces notes, les doublons sont placés dans le sous‑arbre **gauche**, comme dans la page BST.

### Propriétés
- **P1** : Chaque noeud est rouge ou noir.
- **P2** : La racine est noire.
- **P3** : Toutes les feuilles sentinelles (`null`) sont noires.
- **P4** : Un noeud rouge n’a pas de parent rouge (pas deux rouges consécutifs).
- **P5** : Pour tout noeud, chaque chemin vers une feuille contient le même nombre de noeuds noirs (hauteur noire).

Ces propriétés bornent la **hauteur** de l’arbre à environ **2 * log<sub>2</sub>(n+1)** et garantissent des **opérations logarithmiques**. Java utilise ces propriétés pour `TreeMap` et certaines autres structures.

```java
/**
 * noeud d'un arbre rouge-noir.
 * Convention : red == true (ROUGE), red == false (NOIR).
 */
class RBNode {
    private int value;
    private boolean red;
    private RBNode left;
    private RBNode right;
    private RBNode parent;

    public RBNode(int value) {
        this.value = value;
        this.red = true; // On insère rouge, puis on corrige par "fix-up"
    }

    public int getValue() {
        return value;
    }

    public boolean isRed() {
        return red;
    }

    public RBNode getLeft() {
        return left;
    }

    public RBNode getRight() {
        return right;
    }

    public RBNode getParent() {
        return parent;
    }

    public void setValue(int value) {
        this.value = value;
    }

    public void setRed(boolean red) {
        this.red = red;
    }

    public void setLeft(RBNode left) {
        this.left = left;
    }

    public void setRight(RBNode right) {
        this.right = right;
    }

    public void setParent(RBNode parent) {
        this.parent = parent;
    }
}

/**
 * Arbre rouge-noir (Red-Black Tree) — Implémentation minimale.
 * - Insertion en deux phases : BST classique, puis rééquilibrage (fix-up).
 * - Doublons : toujours dirigés vers le SOUS-ARBRE GAUCHE.
 */
class RedBlackTree {

    /**
     * Racine de l'arbre.
     * Invariant P2 : la racine doit être NOIRE après rééquilibrage.
     */
    private RBNode root;
}

```

## Opérations principales

### Insertion
Lorsqu’on insère une valeur dans un arbre rouge-noir, on commence par effectuer une insertion comme dans un **arbre binaire de recherche** : la nouvelle valeur est placée à la position appropriée selon l’ordre. Le nouveau noeud est initialement coloré rouge pour minimiser l’impact sur la hauteur noire. Ensuite, on vérifie les propriétés de l’arbre : si le parent est noir, aucune correction n’est nécessaire. Si le parent est rouge, cela brise la règle P4 « un noeud rouge ne peut pas avoir d’enfant rouge ». On applique alors des rotations et des recolorations selon trois cas principaux :

- **Cas 1** : l’oncle est rouge : recoloration du parent et de l’oncle en noir et du grand-parent en rouge, puis propager vers le haut
- **Cas 2** : l’oncle est noir et le noeud est un enfant “intérieur” : rotation (gauche ou droite) et recoloration

Enfin, la racine est toujours recolorée en noir.

<details markdown="1">
<summary markdown="span">Code d'insertion et rééquilibrage</summary>

```java
/**
 * Insère une valeur dans l'arbre, puis rééquilibre.
 */
public void insert(int value) {
    // 1) Insertion BST classique (sans couleur ni rotation)
    RBNode inserted = bstInsert(value);

    // 2) Rééquilibrage rouge-noir (recoloriage/rotations)
    fixAfterInsertion(inserted);

    // Invariant P2 : la racine doit être noire
    if (root != null) {
        root.setRed(false);
    }
}

/**
 * Insertion style BST : recherche de la position et rattachement.
 * Doublons envoyés à GAUCHE pour cohérence avec nos notes.
 */
private RBNode bstInsert(int value) {
    if (root == null) {
        root = new RBNode(value);
        root.setRed(false); // la racine est toujours noire
        return root;
    }

    RBNode current = root;
    RBNode parent = null;

    while (current != null) {
        parent = current;

        if (value <= current.getValue()) // doublons à GAUCHE
        {
            current = current.getLeft();
        } else if (value > current.getValue()) {
            current = current.getRight();
        }
    }

    RBNode node = new RBNode(value);
    node.setParent(parent);

    if (value <= parent.getValue()) {
        parent.setLeft(node);
    } else {
        parent.setRight(node);
    }

    return node;
}

/**
 * Rééquilibrage après insertion (fix-up) pour restaurer les propriétés :
 * - P4 : pas de deux ROUGES consécutifs (le parent d’un noeud rouge ne peut pas
 * être rouge).
 * - P5 : même nombre de noeuds noirs sur tous les chemins (hauteur noire).
 *
 * Stratégie :
 * - Oncle ROUGE -> recoloriage (parent + oncle NOIRS, grand-parent ROUGE), puis
 * REMONTÉE.
 * - Oncle NOIR -> ROTATIONS (gauche/droite) + recoloriage selon les cas
 * "triangle"/"ligne".
 */
private void fixAfterInsertion(RBNode x) {
    while (x != null && x.getParent() != null && x.getParent().isRed()) {
        RBNode parent = x.getParent();
        RBNode grand = parent.getParent();

        // Parent est l'enfant GAUCHE du grand-parent
        if (grand != null && parent == grand.getLeft()) {
            RBNode uncle = grand.getRight();

            // CAS 1 : oncle ROUGE -> recoloriage + remontée
            if (uncle != null && uncle.isRed()) {
                parent.setRed(false); // parent devient NOIR
                uncle.setRed(false); // oncle devient NOIR
                grand.setRed(true); // grand-parent devient ROUGE
                x = grand; // remonter d'un niveau pour vérifier
            } else {
                // CAS 2 : oncle NOIR -> rotations + recoloriage
                // Sous-cas "triangle": x est enfant DROIT du parent -> rotation GAUCHE sur
                // parent
                if (x == parent.getRight()) {
                    rotateLeft(parent);
                    x = parent; // x suit la rotation
                    parent = x.getParent(); // parent mis à jour
                }

                // Sous-cas "ligne": rotation DROITE sur grand-parent + recoloriage
                parent.setRed(false); // parent NOIR
                if (grand != null) {
                    grand.setRed(true); // grand-parent ROUGE
                    rotateRight(grand);
                }
            }
        } else if (grand != null) {
            // Parent est l'enfant DROIT du grand-parent (cas symétrique)
            RBNode uncle = grand.getLeft();

            // CAS 1 : oncle ROUGE -> recoloriage + remontée
            if (uncle != null && uncle.isRed()) {
                parent.setRed(false);
                uncle.setRed(false);
                grand.setRed(true);
                x = grand;
            } else {
                // CAS 2 : oncle NOIR -> rotations + recoloriage
                // Sous-cas "triangle": x est enfant GAUCHE du parent -> rotation DROITE sur
                // parent
                if (x == parent.getLeft()) {
                    rotateRight(parent);
                    x = parent;
                    parent = x.getParent();
                }

                // Sous-cas "ligne": rotation GAUCHE sur grand-parent + recoloriage
                parent.setRed(false);
                grand.setRed(true);
                rotateLeft(grand);
            }
        }
    }
}

/**
 * Rotation GAUCHE autour de x.
 *
 * Avant :
 * x
 * \
 * y
 * / \
 * A B
 *
 * Après :
 * y
 * / \
 * x B
 * / \
 * A (y.left)
 */
private void rotateLeft(RBNode x) {
    RBNode y = x.getRight();

    if (y == null) {
        return; // Pas d'enfant droit -> pas de rotation possible
    }

    // 1) Le sous-arbre gauche de y devient le sous-arbre droit de x
    x.setRight(y.getLeft());
    if (y.getLeft() != null) {
        y.getLeft().setParent(x);
    }

    // 2) Raccorder y au parent de x
    y.setParent(x.getParent());
    if (x.getParent() == null) {
        root = y; // y devient la nouvelle racine
    } else if (x == x.getParent().getLeft()) {
        x.getParent().setLeft(y);
    } else {
        x.getParent().setRight(y);
    }

    // 3) Finaliser : x devient l'enfant GAUCHE de y
    y.setLeft(x);
    x.setParent(y);
}

/**
 * Rotation DROITE autour de x (symétrique de rotateLeft).
 *
 * Avant :
 * x
 * /
 * y
 * / \
 * A B
 *
 * Après :
 * y
 * / \
 * A x
 * / \
 * B (x.right)
 */
private void rotateRight(RBNode x) {
    RBNode y = x.getLeft();

    if (y == null) {
        return; // Pas d'enfant gauche -> pas de rotation possible
    }

    // 1) Le sous-arbre droit de y devient le sous-arbre gauche de x
    x.setLeft(y.getRight());
    if (y.getRight() != null) {
        y.getRight().setParent(x);
    }

    // 2) Raccorder y au parent de x
    y.setParent(x.getParent());
    if (x.getParent() == null) {
        root = y; // y devient la nouvelle racine
    } else if (x == x.getParent().getRight()) {
        x.getParent().setRight(y);
    } else {
        x.getParent().setLeft(y);
    }

    // 3) Finaliser : x devient l'enfant DROIT de y
    y.setRight(x);
    x.setParent(y);
}
```
</details>

### Suppression
La suppression commence par retirer le noeud comme dans un arbre binaire de recherche. Si le noeud supprimé ou son remplaçant est rouge, aucune violation n’apparaît. Si un noeud noir est supprimé, cela peut réduire la hauteur noire d’un chemin, ce qui brise la propriété d’équilibre **P5**. Pour corriger cela, on introduit la notion de noeud doublement noir et on applique des ajustements :

- **Cas 1** : le frère est rouge : rotation et recoloration pour obtenir un frère noir.
- **Cas 2** : le frère est noir avec deux enfants noirs : recoloration du frère en route et propagation vers le haut
- **Cas 3** : le frère est noir avec un enfant rouge : rotation et recoloration pour restaurer les propriétés.

Ces étapes garantissent que tous les chemins retrouvent la même hauteur noire. La racine est toujours noire à la fin.

<details markdown="1">
<summary markdown="span">Code de suppression et rééquilibrage</summary>

```java
/**
 * Supprime la première occurrence de la valeur dans l'arbre rouge-noir.
 * - Recherche BST de la valeur.
 * - Suppression avec rééquilibrage (fix-after-deletion).
 */
public void delete(int value) {
    RBNode z = search(value);

    if (z != null) {
        deleteNode(z);
    }
}

/**
 * Suppression d'un noeud z :
 * - Si z a 0 ou 1 enfant : on le remplace directement par son enfant.
 * - Si z a 2 enfants : on utilise le SUCCESSEUR (minimum du sous-arbre droit),
 * on transplante z avec le successeur y, et on rattache les sous-arbres de z
 * sous y.
 *
 * Ensuite :
 * - Si le noeud physiquement retiré (y, selon CLRS) était NOIR,
 * on déclenche le fix-after-deletion pour corriger l'éventuel "double noir".
 */
private void deleteNode(RBNode z) {
    RBNode y = z; // noeud physiquement supprimé
    boolean yWasRed = y.isRed(); // couleur originale de y
    RBNode x = null; // enfant qui remonte pour corriger
    RBNode xParent = null; // parent de x (utile si x == null)

    // Cas z n'a pas d'enfant gauche : on remplace z par z.right
    if (z.getLeft() == null) {
        x = z.getRight();
        xParent = z.getParent();
        transplant(z, z.getRight());
    } else if (z.getRight() == null) {
        // Cas z n'a pas d'enfant droit : on remplace z par z.left
        x = z.getLeft();
        xParent = z.getParent();
        transplant(z, z.getLeft());
    } else {
        // Cas général : 2 enfants -> on prend le successeur y (minimum du sous-arbre
        // droit)
        y = treeMinimum(z.getRight());
        yWasRed = y.isRed();
        x = y.getRight(); // Le successeur n'a pas de fils gauche
        xParent = y.getParent();

        // Si le successeur est le fils direct de z, xParent doit pointer sur y
        if (y.getParent() == z) {
            xParent = y;
        } else {
            // Détacher y de sa position et remplacer y par son enfant droit
            transplant(y, y.getRight());

            // Rattacher la "branche droite" de z sous y
            y.setRight(z.getRight());
            if (z.getRight() != null) {
                z.getRight().setParent(y);
            }
        }

        // Remplacer z par y (y prend la place de z)
        transplant(z, y);

        // Rattacher la "branche gauche" de z sous y
        y.setLeft(z.getLeft());
        if (z.getLeft() != null) {
            z.getLeft().setParent(y);
        }

        // y hérite de la couleur de z (pour préserver la hauteur noire avant fix-up)
        y.setRed(z.isRed());
    }

    // Si on a retiré un noeud NOIR, la propriété "hauteur noire égale" peut être
    // violée
    if (!yWasRed) {
        fixAfterDeletion(x, xParent);
    }
}

/**
 * Remplace le sous-arbre enraciné en u par le sous-arbre enraciné en v.
 * (v peut être null) — met à jour les parents.
 */
private void transplant(RBNode u, RBNode v) {
    if (u.getParent() == null) {
        // u était la racine
        root = v;
    } else if (u == u.getParent().getLeft()) {
        u.getParent().setLeft(v);
    } else {
        u.getParent().setRight(v);
    }

    if (v != null) {
        v.setParent(u.getParent());
    }
}

/**
 * Retourne le noeud de valeur minimale du sous-arbre (le plus à gauche).
 */
private RBNode treeMinimum(RBNode node) {
    RBNode current = node;

    while (current != null && current.getLeft() != null) {
        current = current.getLeft();
    }

    return current;
}

/**
 * Fix-after-deletion : corrige le "double noir" après suppression.
 * x : noeud remonté (peut être null) ; xParent : parent de x (utile si x ==
 * null).
 */
private void fixAfterDeletion(RBNode x, RBNode xParent) {
    // Un null est considéré NOIR : (x == null) => "double noir" à corriger
    while ((x != root) && (x == null || !x.isRed())) {
        // x est enfant GAUCHE -> frère à DROITE
        if (xParent != null && x == xParent.getLeft()) {
            RBNode w = xParent.getRight(); // frère

            if (w != null && w.isRed()) {
                // CAS A : frère ROUGE -> recoloriage + rotation pour obtenir un frère NOIR
                w.setRed(false);
                xParent.setRed(true);
                rotateLeft(xParent);
                w = xParent.getRight();
            }

            // Frère NOIR ici (w peut être null, traité comme noir)
            boolean leftIsRed = (w != null && w.getLeft() != null && w.getLeft().isRed());
            boolean rightIsRed = (w != null && w.getRight() != null && w.getRight().isRed());

            if (!leftIsRed && !rightIsRed) {
                // CAS B : frère NOIR avec ENFANTS NOIRS -> colorer frère en ROUGE et remonter
                if (w != null) {
                    w.setRed(true);
                }

                x = xParent;
                xParent = (x != null) ? x.getParent() : null;
            } else {
                // CAS C : frère NOIR avec au moins un enfant ROUGE -> rotations + recoloriage
                if (!rightIsRed) {
                    // Sous-cas : le "bon" enfant n'est pas rouge -> préparer via rotation droite
                    // sur w
                    if (w != null && w.getLeft() != null) {
                        w.getLeft().setRed(false);
                    }

                    if (w != null) {
                        w.setRed(true);
                        rotateRight(w);
                    }

                    w = (xParent != null) ? xParent.getRight() : null;
                }

                // Finalisation : propagation des couleurs et rotation gauche sur le parent
                if (w != null) {
                    w.setRed(xParent != null && xParent.isRed());
                }

                if (xParent != null) {
                    xParent.setRed(false);
                }

                if (w != null && w.getRight() != null) {
                    w.getRight().setRed(false);
                }

                if (xParent != null) {
                    rotateLeft(xParent);
                }

                x = root; // correction terminée
            }
        } else {
            // x est enfant DROIT -> frère à GAUCHE (cas symétrique)
            RBNode w = (xParent != null) ? xParent.getLeft() : null;

            if (w != null && w.isRed()) {
                // CAS A (symétrique) : frère ROUGE
                w.setRed(false);
                if (xParent != null) {
                    xParent.setRed(true);
                    rotateRight(xParent);
                }
                w = (xParent != null) ? xParent.getLeft() : null;
            }

            boolean leftIsRed = (w != null && w.getLeft() != null && w.getLeft().isRed());
            boolean rightIsRed = (w != null && w.getRight() != null && w.getRight().isRed());

            if (!leftIsRed && !rightIsRed) {
                // CAS B (symétrique) : frère NOIR + enfants NOIRS -> recolorer frère en ROUGE
                // et remonter
                if (w != null) {
                    w.setRed(true);
                }

                x = xParent;
                xParent = (x != null) ? x.getParent() : null;
            } else {
                // CAS C (symétrique) : au moins un enfant ROUGE du frère
                if (!leftIsRed) {
                    // Préparer via rotation gauche sur w
                    if (w != null && w.getRight() != null) {
                        w.getRight().setRed(false);
                    }

                    if (w != null) {
                        w.setRed(true);
                        rotateLeft(w);
                    }

                    w = (xParent != null) ? xParent.getLeft() : null;
                }

                if (w != null) {
                    w.setRed(xParent != null && xParent.isRed());
                }

                if (xParent != null) {
                    xParent.setRed(false);
                }

                if (w != null && w.getLeft() != null) {
                    w.getLeft().setRed(false);
                }

                if (xParent != null) {
                    rotateRight(xParent);
                }

                x = root; // correction terminée
            }
        }
    }

    // x (si non null) doit être NOIR à la fin
    if (x != null) {
        x.setRed(false);
    }
}
```
</details>

### Recherche et parcours
La recherche suit le **BST** : **O(log n)**. Les parcours (**infixe**, **préfixe**, **suffixe**) visitent **tous** les noeuds, donc **O(n)**.

---

## Complexité des opérations


| **Opération**  | **Complexité moyenne** | **Complexité pire cas** |
|-----------------|-------------------------|---------------------------|
| Insertion      | O(log n)              | O(log n)                |
| Recherche      | O(log n)              | O(log n)                |
| Suppression    | O(log n)              | O(log n)                |
| Parcours       | O(n)                  | O(n)                    |


{: .highlight}
> Grâce aux propriétés P1–P5, l’arbre rouge‑noir **évite la dégénérescence** des BST et garantit une **hauteur** de l’ordre de **log n**, ce que la JDK exploite dans `TreeMap`.

---

## Applications
- Structures de **sets/maps** triés (`TreeSet`, `TreeMap`).
- **Classements** et index ordonnés.
- Composants internes de bibliothèques nécessitant des garanties de **logarithmicité**.

---

## Liens utiles
- [Visualisation des arbres rouge‑noir](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)
- [Documentation `TreeMap` (JDK 25)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeMap.html)

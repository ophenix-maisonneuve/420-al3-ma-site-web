---
layout: default
title: Condensé
parent: Principes de cryptographie
nav_order: 1
published: true
---

# Condensé (hash)

En créant une **empreinte numérique** des données d'origine, le **condensé** (appelé *hash* en anglais) permet à tous les destinataires de valider que les données reçues sont **identiques aux données d'origine**, c'est-à-dire qu'elles n'ont pas été modifiées.

On appelle cette propriété l'**intégrité des données**.

## Fonction de hachage
Une fonction de hachage est une fonction mathématique prenant en **entrée** des données de **taille variable** et produisant un **résultat de taille fixe**, appelé le **condensé** (*hash*). Il existe plusieurs **algorithmes de hachage** différents, chacun produisant un condensé d'une taille qui lui est propre.

En ordre croissant de sécurité, voici quelques-uns des algorithmes de hachage les plus connus, ainsi que la taille du condensé produit:

### Exemples d’algorithmes
- **MD5** — 128 bits
- **SHA-1** — 160 bits
- **SHA-256** — 256 bits
- **SHA-384** — 384 bits
- **SHA-512** — 512 bits

## Propriétés essentielles

### 1. Déterminisme
Une fonction de hachage doit être **déterministe**, c'est-à-dire qu'une **même entrée doit toujours produire la même sortie**.

Une pratique courante consiste à fournir le condensé avec un fichier partagé. Le destinataire peut ensuite valider l'**intégrité** du fichier reçu en **calculant le condensé** de ce fichier et en le **comparant avec le condensé reçu**. Cette opération n'est possible que si la fonction de hachage est **déterministe**, puisque les deux condensés devront être identiques pour que le fichier soit considéré valide.

Tous les algorithmes énumérés précédemment (*MD5*, *SHA-1*, *SHA-256*, *SHA-384* et *SHA-512*) sont **déterministes**.

### 2. Non-réversibilité
Une fonction de hachage doit être **non-réversible**, c'est-à-dire qu'il doit être **extrêmement difficile** de deviner l'entrée d'origine à partir du condensé.

Une autre utilisation courante des condensés est le **stockage de mots de passe** (par exemple dans le fichier `/etc/shadow` sous Linux). Les mots de passe ne sont pas stockés en clair, mais plutôt sous forme d'un **condensé**. Lorsque l'on doit valider le mot de passe de l'utilisateur, le mot de passe entré est passé dans l'**algorithme de hachage**, et la sortie est **comparée au condensé stocké**. S'ils sont identiques, l'utilisateur est authentifié.

S'il était facile de deviner l'entrée à partir du condensé, cette pratique ne serait pas sécuritaire du tout, car il suffirait de déduire le mot de passe en clair à partir du condensé.

### 3. Résistance aux collisions
Une fonction de hachage doit être **résistante aux collisions**, c'est-à-dire qu'il doit être **extrêmement difficile** de trouver **deux entrées différentes** qui produisent le **même condensé**.

Si un acteur malicieux parvenait à trouver une façon fiable et répétable de **générer des condensés identiques** à partir d'**entrées différentes**, cela lui permettrait théoriquement de remplacer n'importe quel fichier légitime par un fichier de son choix, pouvant être infecté par un **virus** ou autre **logiciel malveillant**.

Le destinataire, constatant que le condensé fourni avec le fichier et le condensé calculé sont **identiques**, croirait à tort qu'il utilise un fichier légitime et exposerait son système à un énorme **risque de sécurité**.

## Génération d’un condensé
L’outil **OpenSSL** permet de calculer un condensé :

```bash
openssl dgst <algorithme> <fichier>
```

Pour écrire le condensé dans un fichier :
```bash
openssl dgst <algorithme> <fichier> > <fichier_hash>
```

Exemple avec SHA-512 :
```bash
openssl dgst -sha512 document.pdf > document.sha
```

{: .highlight}
> En pratique, un condensé seul n'a que très peu de valeur d'un point de vue de sécurité. Il est généralement combiné à une signature numérique afin de valider l'émetteur du fichier à valider et du condensé.

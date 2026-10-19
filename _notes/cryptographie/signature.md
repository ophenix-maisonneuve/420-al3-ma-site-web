---
layout: default
title: Signature numérique
parent: Principes de cryptographie
nav_order: 2
published: true
---

# Signature numérique

La **signature numérique** permet de garantir l’authenticité d’un message et d’attester que son auteur ne peut pas nier l’avoir produit. Elle repose sur l’usage combiné d’une **clé privée** (pour signer) et d’une **clé publique** (pour vérifier).

La signature apporte :
- **Authenticité** — le message provient bien de l’émetteur déclaré.
- **Non‑répudiation** — l’émetteur ne peut pas nier avoir signé le message.

---

## Principe général
La signature se base sur les algorithmes à clés asymétriques (clé privée et clé publique). Pour signer efficacement des données :
1. On calcule d’abord leur **condensé** (*hash*).
2. On **chiffre** ce condensé avec la **clé privée** de l’émetteur.
3. Le destinataire vérifie la signature en :
   - déchiffrant la signature avec la **clé publique**,
   - recalculant le condensé des données reçues,
   - comparant les deux condensés.

{: .highlight}
> Il serait théoriquement possible de signer l'ensemble des données (par exemple un fichier entier ou la totalité d'un courriel), mais cela est rarement le cas en pratique. En effet, les algorithmes par clés asymétriques sont peu performants et mal adaptés pour opérer sur des données de grande taille. C'est pourquoi on signe généralement le condensé que l'on achemine au destinataire avec le contenu à valider.

---

## Algorithmes de signature
Les algorithmes les plus courants sont :
- **RSA** — 2048 bits minimum, 4096 bits recommandé.
- **ECDSA (ECC)** — 256 bits minimum, 512 bits recommandé.
- **DSA** — 2048 bits minimum, 4096 bits recommandé.

Les courbes elliptiques (ECC) sont généralement plus performantes pour une sécurité équivalente.

---

## Création d’une signature
Une signature peut être créée avec l'outil **OpenSSL**.

### 1. Générer une paire de clés
```bash
openssl genpkey -algorithm rsa -out cle_privee.pem -pkeyopt rsa_keygen_bits:4096
openssl rsa -pubout -in cle_privee.pem -out cle_publique.pem
```

### 2. Signer un fichier
```bash
openssl dgst -sha256 -sign cle_privee.pem -out signature.bin document.pdf
```

### 3. Vérifier une signature
```bash
openssl dgst -sha256 -verify cle_publique.pem -signature signature.bin document.pdf
```

---

## Pourquoi utiliser une signature numérique ?
La signature est essentielle dans de nombreux domaines :
- mises à jour logicielles et dépôts de paquets,
- documents légaux envoyés électroniquement,
- certificats X.509 et protocoles comme TLS,
- validation d’intégrité dans les systèmes de fichiers sécurisés.

La combinaison **hachage + signature** constitue la base de la confiance dans les communications modernes.

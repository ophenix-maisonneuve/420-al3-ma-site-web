---
layout: default
title: Chiffrement
parent: Principes de cryptographie
nav_order: 3
published: true
---

# Chiffrement

Le **chiffrement** consiste à appliquer des **transformations mathématiques** à des données en clair afin de les rendre **illisibles** et **irrécupérables** pour quiconque ne possède pas l'information pour effectuer les **transformations inverses**. Cela assure la **confidentialité** des données, car seuls les destinataires désirés posséderont cette information.

Il existe deux principaux types d'algorithmes de chiffrement: ceux utilisant une paire de **clés asymétriques** (**clé publique** et **clé privée**) et ceux utilisant une seule clé (**clé symétrique**).

---

## Chiffrement asymétrique
Le chiffrement asymétrique repose sur une **paire de clés** :
- une **clé publique**, utilisée pour chiffrer les données
- une **clé privée**, gardée secrète et utilisée pour déchiffrer

Ce modèle garantit que seul le destinataire légitime (détenteur de la clé privée) peut accéder aux données. Ce mécanisme est utilisé dans de nombreux protocoles sécurisés, certaines versions de TLS.

### Algorithmes courants
- **RSA** — 2048 bits minimum, 4096 bits pour une sécurité renforcée.
- **ECC / ECIES** — basé sur les courbes elliptiques, offrant une sécurité équivalente à RSA avec des clés bien plus courtes.

{: .warning}
> Le chiffrement asymétrique est **lent** et inadapté pour chiffrer de gros volumes de données. Il est donc généralement utilisé pour :
> - chiffrer de petites données (clés, secrets)
> - établir une session sécurisée
> - échanger une clé symétrique

---

### Chiffrement asymétrique avec OpenSSL
#### 1. Génération d’une paire de clés
```bash
openssl genpkey -algorithm rsa -out cle_privee.pem -pkeyopt rsa_keygen_bits:4096
openssl rsa -pubout -in cle_privee.pem -out cle_publique.pem
```

#### 2. Chiffrement
```bash
openssl pkeyutl -encrypt -inkey cle_publique.pem -pubin -in motdepasse.txt -out motdepasse.enc
```

#### 3. Déchiffrement
```bash
openssl pkeyutl -decrypt -inkey cle_privee.pem -in motdepasse.enc -out motdepasse_dechiffre.txt
```
---

## Chiffrement symétrique
Le chiffrement symétrique utilise **une seule clé** pour chiffrer et déchiffrer. Cette clé peut avoir été partagée entre les interlocuteurs, ou avoir été négociée automatiquement à l'aide de protocoles d'échange de clé comme Diffie-Hellman (ou ses variantes).

Ce type de chiffrement est **rapide**, **efficace** et largement utilisé pour protéger de grandes quantités de données.

### Algorithmes courants
- **AES** — standard moderne, 128 bits minimum, 256 bits recommandé
- **3DES** — encore présent mais de moins en moins recommandé
- **DES** — obsolète et vulnérable

### Chiffrement AES avec OpenSSL
#### 1. Génération d’une clé et d'un vecteur d'initialisation
```bash
openssl rand -hex -out aes.key 32
openssl rand -hex -out aes.iv 16
```

#### 2. Chiffrement d’un fichier
```bash
openssl enc -aes-256-cbc -salt -in document.pdf -out document.enc -iv $(cat aes.iv) -K $(cat aes.key) 
```

#### 3. Déchiffrement
```bash
openssl enc -d -aes-256-cbc -in document.enc -out document_dechiffre.pdf -iv $(cat aes.iv) -K $(cat aes.key)
```

{: .astuce}
> On peut aussi laisser OpenSSL dériver automatiquement la clé AES et le vecteur d'initialisation à partir d'un mot de passe. Il suffit d'utiliser:
> ```bash
> openssl enc -aes-256-cbc -salt -in document.pdf -out document.enc -pass pass:<mot de passe>
> ```

---

## Bonnes pratiques
- Toujours privilégier **AES‑256** pour le chiffrement symétrique.
- Éviter les modes ECB en production ; préférer CBC ou GCM.
- Ne jamais transmettre une clé symétrique en clair (pargager la clé sur un canal sécurisé, ou utiliser Diffie-Hellman ou une variante)
- Utiliser RSA ou ECC uniquement pour les **petites données** ou l’échange de clés.

Le chiffrement constitue l’un des fondements essentiels de la sécurité moderne, utilisé dans les VPN, les serveurs HTTPS, les disques chiffrés et les échanges confidentiels.

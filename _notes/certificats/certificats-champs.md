---
layout: default
title: Champs d’un certificat X.509
parent: Certificats
nav_order: 1
---

# Champs importants d’un certificat X.509

Les certificats X.509 contiennent un ensemble de champs obligatoires et optionnels définis par la norme X.509. Ces champs permettent d’identifier le certificat, de connaître sa validité, son propriétaire, son émetteur et les usages permis.

---

## Version
Le champ **Version** indique la version du standard X.509 utilisée. La version la plus courante aujourd’hui est **v3**, qui permet l’utilisation d’extensions. Les versions 1 et 2 existent encore mais sont rarement utilisées.

![version](../assets/images/certificat-version.png)

---

## Numéro de série
Le **numéro de série** est un identifiant unique attribué par l’autorité de certification au moment de la signature du certificat. Il permet de distinguer un certificat particulier parmi tous ceux émis par la même CA.

![serial](../assets/images/certificat-serial.png)

---

## Période de validité
La validité est définie par deux champs :
- **Not Before** : date à partir de laquelle le certificat devient valide.
- **Not After** : date d’expiration du certificat.

Au‑delà de cette période, le certificat est considéré invalide et doit être renouvelé.

![validite](../assets/images/certificat-validite.png)

---

## Sujet (*Subject*)
Le champ **Subject** décrit l’entité à laquelle le certificat appartient : une personne, une entreprise ou un nom de domaine.

Il peut contenir :
- CN (Common Name),
- O (Organization),
- OU (Organizational Unit),
- C (Country),
- L (Locality),
- ST (State).

![subject](../assets/images/certificat-subject.png)

{: .highlight-title}
> Certificat générique (*wildcard certificate*)
>
> Un certificat peut inclure un nom générique tel que `*.exemple.com`, permettant de sécuriser plusieurs sous-domaines d’une même organisation avec un même certificat.

---

## Noms alternatifs (*Subject Alternative Name – SAN*)
Le champ **subjectAltName (*SAN*)** permet d’associer plusieurs noms de domaine à un même certificat. Cette extension est essentielle pour les certificats web modernes. Les navigateurs se fient désormais davantage au **SAN** qu’au champ **Subject** pour déterminer l’identité du serveur. Un certificat sans **SAN** est souvent refusé par les navigateurs actuels.

---

## Émetteur (*Issuer*)
Le champ **Issuer** identifie l’autorité de certification (*certificate authority, CA*) qui a signé le certificat ou, plus précisément, la requête de signature (*certificate signing request, CSR*).

Les certificats peuvent être signés par :
- une **CA racine**,
- une **CA intermédiaire**,
- ou être **auto‑signés** (utilisés principalement pour des tests).

Les CA intermédiaires forment une **chaîne de certificats** jusqu’à la racine reconnue globalement.

![issuer](../assets/images/certificat-issuer.png)

---

## Clé publique (Subject Public Key Info)
Ce champ contient :
- l’algorithme utilisé (**RSA**, **ECC**, etc.),
- la taille de la clé,
- la clé publique elle‑même.

Il s’agit d’un élément fondamental permettant au client de vérifier les signatures et d’établir des connexions sécurisées.

![public key info](../assets/images/certificat-public-key-info.png)

---

## Algorithme de signature et signature
Deux champs distincts :
- **Signature Algorithm** : indique l’algorithme utilisé (ex : SHA256 avec RSA ou ECDSA),
- **Signature** : contient la signature numérique du certificat, encodée en Base64.

L’algorithme de signature combine un **hachage cryptographique** et un **algorithme à clé publique**.

![signature](../assets/images/certificat-signature.png)

---

**Ces champs constituent la structure essentielle d’un certificat X.509. Ils permettent aux clients (navigateurs, serveurs, applications) de vérifier la légitimité du certificat, d’établir des connexions chiffrées et d’assurer une chaîne de confiance fiable sur un réseau public comme Internet.**

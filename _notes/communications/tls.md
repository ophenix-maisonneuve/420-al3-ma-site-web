---
layout: default
title: TLS
parent: Communications sécurisées
nav_order: 1
---

# TLS (Transport Layer Security)

Le protocole **TLS** est le standard moderne permettant de sécuriser les communications sur Internet. Il assure la **confidentialité**, **l’intégrité** et **l’authenticité** des données échangées entre un client et un serveur. TLS permet d'établir un canal sécurisé afin d'y faire passer les données d'un protocole applicatif. Il est notamment utilisé pour sécuriser les connexions HTTPS, les VPN, les courriels et de nombreuses autres applications réseau. 

---

## Objectifs de TLS
TLS combine plusieurs mécanismes cryptographiques afin de garantir :

- **La confidentialité** : les données sont chiffrées, empêchant toute lecture par un tiers. (Chiffrement symétrique — généralement AES.)
- **L’intégrité** : les messages sont protégés contre les modifications accidentelles ou malveillantes (hachage + MAC).
- **L’authenticité** : le client vérifie l’identité du serveur grâce à un certificat X.509.

Ces trois piliers assurent une communication fiable sur un réseau non sécurisé.

---

## Fonctionnement général du handshake TLS
Le **handshake TLS** est la séquence d’échanges permettant d’établir un canal sécurisé. Lors de ce processus :

1. **Le client contacte le serveur** et propose une liste d’algorithmes (cipher suites).
2. **Le serveur choisit une suite cryptographique** et envoie son **certificat X.509**.
3. **Le client vérifie le certificat** (validité, chaîne de confiance, SAN, etc.).
4. **Un secret partagé** est négocié, généralement via RSA (TLS 1.2 et antérieur seulement), Diffie‑Hellman ou ECDHE.
5. Les deux parties **dérivent une clé symétrique** et **passent en communication chiffrée** à l’aide de cette clé.

Ce processus garantit que seul le client et le serveur peuvent comprendre les données échangées.

---

## Rôle des certificats X.509 dans TLS
Les certificats X.509 sont essentiels dans TLS. Ils permettent au client de :
- confirmer l’identité du serveur
- vérifier la chaîne de confiance, c'est-à-dire la validité de l'autorité (ou des autorités) de certification ayant signé le certificat
- s’assurer de la validité du certificat (dates, numéro de série, extensions)
- valider la correspondance avec le domaine visité (*SAN*).

Si le certificat n’est pas valide ou reconnu, la connexion TLS est interrompue.

---

## Algorithmes utilisés dans TLS
TLS utilise plusieurs familles d’algorithmes :

### 1. **Algorithmes asymétriques**
Utilisés lors du handshake pour l’échange de clés :
- RSA (2048 - 4096 bits)
- ECDHE (courbes elliptiques)

### 2. **Algorithmes symétriques**
Utilisés pour le chiffrement du trafic :
- AES‑128 ou AES‑256 (recommandé)
- 3DES (désuet)

### 3. **Fonctions de hachage**
Utilisées pour l’intégrité (HMAC) :
- SHA‑256
- SHA‑384
- SHA‑512

---

## Avantages de TLS
- Protège les données contre les interceptions.
- Empêche les attaques *man‑in‑the‑middle* grâce aux certificats.
- Permet une authentification forte.
- Permet des algorithmes modernes et extensibles.
- Permet d'encapsuler du trafic applicatif; autrement dit, on peut encapsuler presque tous les protocoles applicatifs dans une session TLS afin de les sécuriser.

---

## Limites
- Dépend des autorités de certification : un certificat compromis met en danger de nombreux services.
- Peut être vulnérable si les paramètres cryptographiques sont faibles : vieux protocoles (SSL), clés RSA courtes, utilisation de SHA‑1, etc.

---

## Conclusion
TLS est ajourd'hui le protocole le plus utilsé pour assurer des communications sécurisées. Grâce à ses mécanismes de chiffrement, d’intégrité et d’authentification, il protège la majorité des échanges numériques modernes, du simple site web aux infrastructures critiques.

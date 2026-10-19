---
layout: default
title: HTTPS
parent: Communications sécurisées
nav_order: 2
---

# HTTPS

**HTTPS (HyperText Transfer Protocol Secure)** est la version sécurisée du protocole HTTP. Contrairement à HTTP, qui transmet les données en clair, HTTPS encapsule le trafic dans une connexion **TLS**, garantissant la confidentialité, l'intégrité et l'authenticité des échanges.

---

## Pourquoi HTTPS ?
Sans protection, les données circulant via HTTP peuvent être interceptées, lues, modifiées ou utilisées dans des attaques de type *man‑in‑the‑middle*. HTTPS résout ces problèmes grâce à :
- **la confidentialité** via le chiffrement TLS
- **l’intégrité** assurée par des fonctions de hachage et des MAC (*message authentication code*)
- **l’authentification** garantie par un certificat X.509 présenté par le serveur.

HTTPS est aujourd’hui indispensable pour tout service web manipulant des données sensibles ou personnelles.

---

## Fonctionnement
HTTPS peut être vu comme :

```
HTTPS = HTTP + TLS
```

Le navigateur établit d’abord une connexion TLS avec le serveur. Une fois le canal sécurisé créé :
- les requêtes HTTP sont envoyées à l’intérieur du tunnel TLS
- tout le contenu (URL, cookies, en‑têtes, corps) est chiffré
- un attaquant ne peut ni lire ni modifier les données.

Le port standard de HTTPS est **443**.

---

## Rôle du certificat X.509 dans HTTPS
Lors de l'établissement de la connexion :
1. le serveur envoie son certificat X.509
2. le client vérifie sa validité (dates, signature, chaîne de confiance)
3. le client vérifie la correspondance du domaine (SAN ou CN)
4. une clé de session est négociée pour chiffrer la communication.

Si le certificat est invalide, expiré ou auto‑signé, le navigateur émet une alerte.

---

## Protection contre les attaques MITM
Avec HTTPS :
- un attaquant ne peut pas usurper l'identité du serveur sans un certificat valide
- un attaquant ne peut pas modifier les données sans invalider l'intégrité
- toute tentative d’interception entraîne une erreur TLS

HTTPS protège ainsi contre la majorité des attaques sur les réseaux publics.

---

## Bonnes pratiques
Pour maximiser la sécurité :
- utiliser un certificat signé par une CA reconnue
- éviter les algorithmes cryptographiques obsolètes (*cipher suites* en anglais) (RSA 1024, SHA‑1, 3DES)
- activer **HSTS** (*HTTP String Transport Security*)pour forcer l’usage de HTTPS
- rediriger tout le trafic HTTP vers HTTPS
- renouveler régulièrement les certificats

---

## Conclusion
HTTPS est essentiel au fonctionnement sécurisé du web moderne. En combinant HTTP et TLS, il garantit un échange chiffré, authentifié et intègre, empêchant toute interception ou manipulation des données en transit via des attaques de type **personne dans le milieu** (*man-in-the-middle* ou *MITM attack*).

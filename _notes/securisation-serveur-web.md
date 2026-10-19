---
layout: default
title: Sécurisation d'un serveur web
nav_order: 4
has_children: true
has_toc: true
published: true
---

# Sécurisation d'un serveur web

La sécurisation d’un serveur web repose sur l’utilisation de **certificats X.509**, de configurations TLS appropriées et de bonnes pratiques d’administration. Elle vise à assurer la **confidentialité**, **l’intégrité** et **l’authenticité** des communications entre un client et un serveur.

## Rôle du certificat X.509
Lorsqu’un serveur web utilise HTTPS, il présente un **certificat X.509** au client. Celui‑ci permet :
- d’authentifier l’identité du serveur
- d’établir une clé de session sécurisée
- d’assurer une communication chiffrée via TLS

Le certificat peut être :
- **auto‑signé** (utilisé pour des tests)
- signé par une **autorité locale** (CA interne)
- signé par une **autorité reconnue** (recommandé pour la production)

## Bonnes pratiques de configuration HTTPS
Pour sécuriser un serveur web :
- utiliser des certificats valides et à jour
- désactiver les protocoles obsolètes (ex.: SSLv2, SSLv3)
- éviter les algorithmes cryptographiques faibles (ex.: RSA < 2048 bits, SHA‑1, 3DES)
- forcer la redirection de HTTP vers HTTPS
- limiter les accès aux clés privées via des permissions strictes sur les systèmes de fichiers
- activer **HSTS** (*HTTP String Transport Security*)pour forcer l’usage de HTTPS

## Déploiement sur un serveur web
Les sections suivantes décrivent la mise en place et l’activation d’HTTPS pour **Nginx** et **Apache**, incluant :
- l’installation du serveur
- l’ajout du certificat et de la clé privée
- la configuration d’un hôte virtuel HTTPS
- la redirection de HTTP vers HTTPS
- l’activation du site.

---

La sécurisation d’un serveur web est une étape indispensable pour la protection des services en ligne. Les sections suivantes détaillent les configurations nécessaires pour **Nginx** et **Apache**, deux des serveurs web les plus utilisés.

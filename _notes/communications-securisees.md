---
layout: default
title: Communications sécurisées
nav_order: 3
has_children: true
has_toc: true
published: true
---

# Communications sécurisées

Les communications sécurisées désignent l’ensemble des mécanismes permettant de protéger l’échange de données entre deux systèmes. Elles reposent sur des protocoles comme **TLS** et **HTTPS**, qui combinent chiffrement, authentification et intégrité afin de garantir une transmission fiable sur des réseaux publics.

## Pourquoi sécuriser les communications ?
Les données circulant sur un réseau peuvent être interceptées, modifiées ou usurpées. Les communications sécurisées répondent donc à trois besoins :
- **Confidentialité** : empêcher un tiers de lire les données échangées.
- **Intégrité** : vérifier que les données n’ont pas été altérées.
- **Authenticité** : s’assurer de l’identité de l'interlocuteur avec lequel on communique.

Les protocoles modernes, tels que TLS, combinent ces mécanismes pour fournir un canal sécurisé.

## TLS
Le protocole **TLS (Transport Layer Security)** est utilisé pour sécuriser les communications applicatives (web, courriel, VPN, etc.). Il offre :
- un **chiffrement** (généralement AES ou un algorithme similaire)
- une **authentification** via des certificats X.509
- une **intégrité** via des fonctions de hachage et des MAC (*message authentication code*)

TLS est souvent utilisé comme canal pour encapsuler des protocoles applicatifs afin de les sécuriser. TLS est notamment utilisé pour établir la couche sécurisée de HTTPS.

## HTTPS
**HTTPS** est simplement la version sécurisée du protocole HTTP. Le trafic HTTP est encapsulé dans une session TLS, assurant :
- la protection du contenu,
- la vérification de l’identité du serveur grâce au certificat,
- la mise en place d’un canal de communication chiffré.

HTTPS est aujourd’hui indispensable pour les sites web modernes.

## Les certificats X.509 dans TLS
Lors de l’établissement d’une connexion TLS, le serveur transmet son **certificat X.509** au client. Celui-ci est utilisé pour :
- authentifier l’identité du serveur
- établir une clé de session sécurisée
- construire une chaîne de confiance jusqu’à une autorité de certification racine.

La validité, le numéro de série, les extensions et les valeurs du champ SAN jouent ici un rôle crucial.

## Résumé
Les communications sécurisées reposent sur trois piliers cryptographiques et utilisent des certificats pour authentifier les serveurs, tandis que TLS et HTTPS assurent un tunnel chiffré fiable pour les échanges. Sans ces mécanismes, les données échangées seraient vulnérables aux interceptions et manipulations.

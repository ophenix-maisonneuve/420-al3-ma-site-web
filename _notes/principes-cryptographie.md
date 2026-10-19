---
layout: default
title: Principes de cryptographie
nav_order: 1
has_children: true
has_toc: true
published: true
---

# Principes de cryptographie

La cryptographie repose sur trois piliers fondamentaux : l'**intégrité**, l'**authenticité** et la **confidentialité**. Ces notions sont souvent combinées dans des protocoles populaires comme TLS et SSH afin d'assurer une communication sécurisée.

## Intégrité
L'intégrité garantit que les données **n'ont pas été modifiées ou trafiquées**. On utilise pour cela des fonctions de hachage produisant une empreinte unique et de taille fixe aussi appelée **condensé**.

## Authenticité
L'authenticité assure que les données **proviennent bien de l'émetteur légitime**. Elle repose généralement sur l'utilisation d'une **signature numérique** créée avec une **clé privée** et vérifiée avec la **clé publique** correspondante.

## Confidentialité
La confidentialité **empêche** des tiers non autorisés de **lire des données**. Elle repose sur le **chiffrement**, qu'il soit **symétrique** (même clé pour chiffrer/déchiffrer) ou **asymétrique** (paire clé publique/clé privée).

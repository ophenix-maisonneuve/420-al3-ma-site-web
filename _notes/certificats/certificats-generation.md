---
layout: default
title: Génération de CSR et de certificats
parent: Certificats
nav_order: 2
---

# Génération de CSR et de certificats

La création d’un certificat X.509 commence par la génération d’une **clé privée**, suivie de la création d’une **requête de signature de certificat** (*CSR — Certificate Signing Request*). La *CSR* est ensuite soit **auto‑signée**, soit transmise à une **autorité de certification** (*CA*) pour obtenir un certificat officiel.

---

## 1. Génération d’une clé privée
Une clé privée constitue la base d’un certificat. Elle peut être générée avec OpenSSL :
```bash
openssl genpkey -algorithm rsa -out cle_privee.pem -pkeyopt rsa_keygen_bits:4096
```
Cette commande crée une clé privée RSA de 4096 bits. D’autres algorithmes comme **ECC** peuvent aussi être utilisés.

---

## 2. Création de la requête de signature (*CSR*)
Une *CSR* contient :
- la clé publique dérivée de la clé privée,
- les informations d’identification,
- les extensions demandées (comme les *SAN*),
- une signature réalisée avec la clé privée.

Exemple de création d’un *CSR* :
```bash
openssl req -new -key cle_privee.pem -out certificat.csr -addext "subjectAltName = DNS:monserveur.com"
```
Les informations (CN, O, OU, etc.) sont ensuite demandées dans l’invite OpenSSL.

---

## 3. Visualisation du CSR
Pour inspecter un CSR :
```bash
openssl req -in certificat.csr -text -noout
```
Un CSR ne contient **ni numéro de série, ni période de validité, ni signature finale** : ces éléments sont ajoutés par la CA lors de la signature.

---

## 4. Création d’un certificat auto‑signé
Pour générer un certificat auto‑signé :
```bash
openssl x509 -req -in certificat.csr -signkey cle_privee.pem -out certificat.crt -days 365
```
Ce certificat est fonctionnel, mais n’est généralement pas reconnu par les navigateurs car il n’est signé par aucune CA reconnue.

---

## 5. Création d’un certificat auto‑signé en une commande
OpenSSL permet aussi de créer un certificat auto‑signé d’un seul coup :
```bash
openssl req -x509 -new -nodes -key cle_privee.pem -sha256 -days 365 -out certificat.crt -addext "subjectAltName = DNS:monserveur.com"
```
Cette approche facilite les tests rapides ou les environnements internes.

---

## 6. Création d’une autorité de certification (*CA*)
Pour émettre des certificats, il est possible de créer une autorité de certification locale :
```bash
openssl genpkey -algorithm rsa -out ca_cle_privee.pem -pkeyopt rsa_keygen_bits:4096
```
Puis créer son certificat auto‑signé avec les extensions CA v3 :
```bash
openssl req -x509 -new -nodes -key ca_cle_privee.pem -sha256 -days 365 -out ca_certificat.crt -addext basicConstraints=CA:TRUE -addext keyUsage=digitalSignature,cRLSign,keyCertSign
```
Ces extensions définissent le certificat comme une autorité capable de signer d’autres certificats.

---

## 7. Signature d’un certificat avec une CA
Une fois la *CA* prête, elle peut signer un *CSR* :
```bash
openssl x509 -req -in certificat.csr -CA ca_certificat.crt -CAkey ca_cle_privee.pem -CAcreateserial -out certificat_signe.crt -days 365 -sha256 -copy_extensions copyall
```
Cette commande :
- signe la CSR,
- crée un numéro de série,
- applique les extensions demandées,
- génère un certificat final valide.


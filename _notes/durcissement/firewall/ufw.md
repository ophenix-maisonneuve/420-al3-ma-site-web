---
layout: default
title: "UFW"
parent: "Pare-feu Linux"
nav_order: 2
---

# UFW

**UFW** (*Uncomplicated Firewall*) est un utilitaire bâti en tant que couche d'abstraction au-dessus de `iptables` permettant de configurer simplement le pare-feu Linux. Comme son nom l'indique, il vise d'abord la simplicité et la lisibilité, sacrifiant parfois des fonctionnalités plus avancées offertes par d'autres pare-feux.

Traditionnellement, la configuration de règles de pare-feu avec `iptables` nécessitait la création de nombreuses règles pouvant rapidement devenir difficiles à lire et à maintenir. UFW a été conçu afin de simplifier cette tâche tout en conservant le plus possible la puissance du moteur de filtrage sous-jacent.

UFW est particulièrement populaire sur les serveurs Linux de petite et moyenne taille ainsi que sur les postes de travail. Il est installé par défaut sur plusieurs distributions dérivées de Debian, notamment Ubuntu.

---

## Installation
Sur les distributions basées sur Debian, UFW peut être installé à l'aide de la commande suivante :

```bash
sudo apt install ufw
```

Une fois installé, le service demeure inactif jusqu'à ce qu'il soit explicitement activé.

---

## Fonctionnement général

UFW filtre les paquets réseau selon un ensemble de règles. Lorsqu'un paquet est reçu ou envoyé, le pare-feu parcourt les règles afin de déterminer l'action à effectuer.

Les actions les plus courantes sont :

| Action | Description |
|----------|-------------|
| allow | Autorise le trafic |
| deny | Bloque le trafic sans réponse |
| reject | Bloque le trafic en retournant un message de refus |

Une règle peut également préciser :

- la direction du trafic (entrant ou sortant)
- l'adresse IP source ou destination
- un sous-réseau
- une interface réseau
- un protocole
- un ou plusieurs ports (ou intervalle de ports)

## Commandes UFW

Une commande UFW prend la forme suivante :
```bash
ufw [ACTION] <COMMANDE> [DIRECTION] [CRITÈRES]
```

où

```

ufw
├─ ACTION (optionnelle; si absente : ajout en fin de liste)
│   ├─ insert NUM (insertion à la ligne NUM)
│   ├─ delete NUM (suppression à la ligne NUM)
│   └─ prepend    (ajout en tête de liste)
│
├─ COMMANDE
│   ├─ allow
│   ├─ deny
│   ├─ reject
│
├─ DIRECTION (optionnelle)
│   ├─ in
│   └─ out
│
└─ CRITÈRES (lisibles, proches du langage naturel)
    ├─ interface      (in/out on ens33)
    ├─ adresse IP     (to/from 192.168.1.100, any)
    ├─ sous-réseau    (to/from 192.168.1.0/24, any)
    ├─ service        (to/from port ssh, http, https)
    ├─ port           (to/from port 22, 80, 443)
    └─ plage de ports (to/from port 6000:6007/tcp)

```

## Politiques par défaut

Avant d'ajouter des règles spécifiques, il est recommandé de définir les politiques par défaut. Une approche courante, consiste à *bloquer tout le trafic entrant* et à *autoriser tout le trafic sortant*. 

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

{: .warning}
> Bien que très courante, cette configuration n'est toutefois pas la plus sécuritaire, car elle permet toujours les attaques de type *reverse-shell* où du code malveillant établit installé au préalable établit la connexion vers un site malveillant. 

---

## Autorisation de services

UFW permet de spécifier plusieurs services courants par leur nom. Par exemple, pour autoriser le protocole SSH :

```bash
sudo ufw allow ssh
```

ou, de façon équivalente avec le numéro du port :

```bash
sudo ufw allow 22
```

Pour un serveur web : 

```bash
sudo ufw allow http
sudo ufw allow https
```

Ces commandes autorisent respectivement les ports 80 et 443.

---

## Gestion des ports

### Port unique

Autoriser un port spécifique :

```bash
sudo ufw allow 3306
```

Exemple : accès à un serveur MySQL.

### Plage de ports

Certaines applications utilisent plusieurs ports consécutifs.

```bash
sudo ufw allow 6000:6007/tcp
```

Cette règle autorise les connexions TCP sur tous les ports de 6000 à 6007 inclusivement.

---

## Filtrage par adresse IP

Il est souvent souhaitable de limiter l'accès à un service à certaines machines seulement.

### Adresse IP unique

Autoriser tous les services à une adresse :

```bash
sudo ufw allow from 192.168.1.100
```

### Accès à un port spécifique

Autoriser SSH uniquement à une adresse :

```bash
sudo ufw allow from 192.168.1.100 to any port 22
```

### Sous-réseau complet

Autoriser l'accès à SSH à tout le réseau local :

```bash
sudo ufw allow from 192.168.1.0/24 to any port 22
```

Cette approche est fréquemment utilisée pour limiter l'accès administratif à un réseau interne.

---

## Filtrage par interface réseau

Sur un serveur possédant plusieurs interfaces, il est possible de limiter une règle à une interface particulière.

Exemple :

```bash
sudo ufw allow in on ens33 to any port 80
```

Cette règle autorise les connexions HTTP reçues sur l'interface **ens33** seulement.

---

## Gestion des règles

### Affichage des règles

Afficher les règles actuellement configurées :

```bash
sudo ufw status
```

Afficher davantage d'informations :

```bash
sudo ufw status verbose
```

### Affichage avec numéros

Pour faciliter la suppression de règles :

```bash
sudo ufw status numbered
```

Exemple :

```text
[1] 22/tcp ALLOW IN Anywhere
[2] 80/tcp ALLOW IN Anywhere
[3] 443/tcp ALLOW IN Anywhere
```

### Suppression d'une règle

```bash
sudo ufw delete 2
```

La deuxième règle sera retirée de la configuration.

---

## Activation du pare-feu

Une fois la configuration terminée, le pare-feu peut être activé :

```bash
sudo ufw enable
```

### Remarque

Lorsqu'on travaille à distance via SSH, il est fortement recommandé d'autoriser SSH avant d'activer UFW.

Par exemple :

```bash
sudo ufw allow ssh
sudo ufw enable
```

Dans le cas contraire, le serveur pourrait devenir inaccessible à distance.
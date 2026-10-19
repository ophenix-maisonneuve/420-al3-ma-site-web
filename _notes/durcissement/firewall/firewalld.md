---
layout: default
title: "firewalld"
parent: "Pare-feu Linux"
nav_order: 3
---

# firewalld

**firewalld** est un pare-feu utilisé principalement dans l'écosystème Red Hat (RHEL, Rocky Linux, AlmaLinux, Fedora, etc.). Comme UFW, il agit comme une couche d'abstraction permettant de simplifier la configuration du pare-feu Linux sans avoir à manipuler directement nftables ou iptables. 

Firewalld introduit la notion de **zones**, permettant d'appliquer différents niveaux de confiance selon différents critères (interface réseau spécifique, sous-réseaux, etc.). Cette approche est particulièrement utile sur les serveurs possédant plusieurs interfaces ou lorsqu'un système est connecté simultanément à différents réseaux.

Contrairement à d'autres pare-feux comme UFW, firewalld possède deux configurations : la configuration active en mémoire (*runtime configuration*) et la configuration permanente (*permanent configuration*). Cela permet aux administrateurs d'effectuer des tests rapidement en mémoire et de les annuler facilement si quelque chose ne fonctionne pas.

## Installation

Sur les distributions de la famille Red Hat, firewalld est généralement installé par défaut.

S'il ne l'est pas :

```bash
sudo dnf install firewalld
```

Démarrer et activer le service :

```bash
sudo systemctl enable --now firewalld
```

Vérifier son état :

```bash
sudo systemctl status firewalld
```

## Fonctionnement général

Comme tout pare-feu, firewalld filtre les paquets réseau en fonction d'un ensemble de règles en tenant toujours compte de l'ordre dans lequel ces règles sont définies.

Les actions les plus courantes sont :

| Action | Description |
|---------|-------------|
| allow | Autorise le trafic |
| deny | Bloque le trafic |
| reject | Bloque le trafic et retourne une réponse au client |

Une règle peut notamment tenir compte :

- de la zone concernée
- de l'adresse IP source
- d'un sous-réseau
- d'une interface réseau
- d'un protocole
- d'un service
- d'un port
- de conditions plus avancées à l'aide des *rich rules*

## Commande firewall-cmd

L'outil utilisé pour configurer `firewalld` est `firewall-cmd`. Une commande `firewall-cmd` prend généralement la forme suivante :

```bash
firewall-cmd [CONTEXTE] <ACTION> [PARAMÈTRES]
```

où :

```
firewall-cmd
├─ CONTEXTE (optionnel)
│   ├─ --zone=<zone>
│   └─ --permanent
│
├─ ACTION
│   ├─ --add-service=<service>
│   ├─ --remove-service=<service>
│   ├─ --add-port=<port/protocole>
│   ├─ --remove-port=<port/protocole>
│   ├─ --add-rich-rule='<règle>'
│   ├─ --remove-rich-rule='<règle>'
│   ├─ --list-services
│   ├─ --list-ports
│   ├─ --list-rich-rules
│   └─ --list-all
│
└─ PARAMÈTRES
    ├─ service      (ssh, http, https, dns, ...)
    ├─ port         (22/tcp, 443/tcp, ...)
    ├─ plage        (6000-6007/tcp)
    ├─ interface    (ens33, eth0, ...)
    ├─ zone         (public, home, work, trusted, ...)
    └─ rich rule
        ├─ source address="192.168.1.100"
        ├─ source address="192.168.1.0/24"
        ├─ destination address="..."
        ├─ service name="ssh"
        ├─ port port="22" protocol="tcp"
        └─ action
            ├─ accept
            ├─ reject
            └─ drop
```

## Zones

Une zone représente un niveau de confiance appliqué à une interface réseau ou à une source donnée. Chaque zone possède ses propres règles. Par exemple, pour un serveur possédant :

- une interface connectée à Internet
- une interface connectée à un réseau local privé

Il est alors possible d'appliquer des règles plus restrictives sur l'interface publique et des règles plus permissives sur l'interface interne.

### Zones courantes

Firewalld vient par défaut avec un certain nombre de zone prédéfinies, chacune ayant un niveau de confiance différent par défaut :

| Zone | Description |
|------|-------------|
| drop | Rejette tout le trafic entrant sans réponse |
| block | Bloque le trafic entrant et retourne un refus |
| public | Zone utilisée pour les réseaux non fiables |
| home | Zone destinée à un réseau domestique |
| work | Zone destinée à un environnement professionnel |
| trusted | Autorise pratiquement tout le trafic |

### Création d'une zone

Si l'on désire créer une zone personnalisée plutôt que de travailler avec l'une des zones prédéfinies, il est possible d'en créer une nouvelle :

```bash
sudo firewall-cmd --permanent --new-zone=serveurs-web
```

Cette zone sera ensuite utilisable de la même manière que toutes les zones prédéfinies.

### Affichage des zones

Pour afficher les zones disponibles :

```bash
sudo firewall-cmd --get-zones
```

Pour afficher la zone par défaut :

```bash
sudo firewall-cmd --get-default-zone
```

Pour afficher les règles d'une zone :

```bash
sudo firewall-cmd [--zone=<nom de la zone>] --list-all
```


### Modification de la zone par défaut

```bash
sudo firewall-cmd --set-default-zone=public
```

### Association à une zone

Pour associer tout le trafic d'une interface spécifique à une zone :

```bash
sudo firewall-cmd --permanent --zone=public --add-interface=ens33
```

> Une fois cette commande exécutée, les règles de la zone *public* s'appliqueront automatiquement à l'interface **ens33**.

Pour associer le trafic provenant d'un sous-réseau spécifique à une zone : 

```bash
sudo firewall-cmd --permanent --zone=work --add-source=192.168.1.0/24
```
> Une fois cette commande exécutée, les règles de la zone *work* s'appliqueront automatiquement à tout le trafic provenant du sous-réseau 192.168.1.0/24.


## Politiques par défaut

Chaque zone définit son `target`, qui correspond à la politique qui sera appliquée par défaut si aucune règle spécifique ne s'applique. Il s'agit de l'équivalent de la `policy` pour une chaîne sous `iptables`. Habituellement, il n'est pas nécessaire de modifier le `target` des zones prédéfinies, car il a généralement la valeur `default`. Cela peut toutefois être très utile pour une zone personnalisée.

{: .astuce}
> La valeur `default` ne correspond pas à une politique spécifique telle que `ACCEPT`, `DROP` ou `REJECT`. Elle indique plutôt à firewalld d'utiliser le comportement normal prévu pour la zone. Pour cette raison, les zones prédéfinies utilisent généralement le *target* `default` et on le change rarement.


Pour modifier la politique par défaut pour une zone :

```bash
sudo firewall-cmd --permanent --zone=serveurs-web --set-target=DROP
```
> La commande ci-haut refusera tout le trafic pour la zone `serveurs-web` à moins qu'une règle ne l'autorise explicitement.


## Configuration des services

Comme UFW, firewalld reconnaît plusieurs services courants.

Autoriser SSH :

```bash
sudo firewall-cmd [--permanent] --add-service=ssh
```

Autoriser HTTP :

```bash
sudo firewall-cmd [--permanent] --add-service=http
```

Autoriser HTTPS :

```bash
sudo firewall-cmd [--permanent] --add-service=https
```

Recharger la configuration :

```bash
sudo firewall-cmd --reload
```

### Remarque

Contrairement à UFW, plusieurs commandes de firewalld nécessitent l'option :

```bash
--permanent
```

pour être conservées après un redémarrage ou un rechargement du service.

Sans cette option, la modification est appliquée uniquement à la configuration active (en mémoire)

## Gestion des ports

### Port unique

Autoriser un port :

```bash
sudo firewall-cmd [--permanent] --add-port=3306/tcp
```

### Plage de ports

Autoriser plusieurs ports :

```bash
sudo firewall-cmd [--permanent] --add-port=6000-6007/tcp
```

Une fois les modifications effectuées :

```bash
sudo firewall-cmd --reload
```

## Filtrage par adresse IP

Avec firewalld, il existe deux approches principales pour contrôler l'accès selon l'adresse IP. 

La première consiste à associer les hôtes ou sous-réseaux à différentes zones à l'aide de `--add-source`...

```bash
```bash
sudo firewall-cmd --permanent --zone=work --add-source=192.168.1.0/24
```

... puis à recharger la configuration :

```bash
sudo firewall-cmd --reload
```

La deuxième consiste à utiliser des *rich rules* lorsque des exceptions ou des conditions plus précises sont nécessaires (voir prochaine section).

## Rich Rules

Les **rich rules** permettent de créer des règles avancées dépassant les simples ouvertures de ports ou de services. On peut les voir comme l'équivalent des règles UFW telles que :

```bash
sudo ufw allow from 192.168.1.100 to any port 22
```

ou

```bash
sudo ufw deny from 10.0.0.0/24 to any port 80
```

Une rich rule suit généralement la structure :

```text
rule [conditions] [action]
```

où :

```text
rule
├─ family="ipv4" ou "ipv6"
├─ source address="..."
├─ destination address="..."
├─ service name="..."
├─ port port="..." protocol="..."
└─ action
    ├─ accept
    ├─ reject
    └─ drop
```

### Exemple : autoriser SSH

```bash
sudo firewall-cmd \
    --permanent \
    --add-rich-rule='rule family="ipv4" source address="192.168.1.100" service name="ssh" accept'
```

### Exemple : bloquer une adresse

```bash
sudo firewall-cmd \
    --permanent \
    --add-rich-rule='rule family="ipv4" source address="203.0.113.50" drop'
```

### Exemple : limiter l'accès HTTP à un sous-réseau

```bash
sudo firewall-cmd \
    --permanent \
    --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" service name="http" accept'
```

La logique est donc très similaire à UFW.

## Gestion des règles

### Affichage des services autorisés

```bash
sudo firewall-cmd --list-services
```

### Affichage des ports autorisés

```bash
sudo firewall-cmd --list-ports
```

### Affichage des rich rules

```bash
sudo firewall-cmd --list-rich-rules
```

### Affichage complet

```bash
sudo firewall-cmd --list-all
```

### Suppression d'une règle

Supprimer un service :

```bash
sudo firewall-cmd --permanent --remove-service=http
```

Supprimer un port :

```bash
sudo firewall-cmd --permanent --remove-port=3306/tcp
```

Après toute modification :

```bash
sudo firewall-cmd --reload
```
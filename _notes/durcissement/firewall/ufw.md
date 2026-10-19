---
layout: default
title: "UFW"
parent: "Pare-feu Linux"
nav_order: 2
---

# UFW

**UFW** (*Uncomplicated Firewall*) est un utilitaire bâti sur iptables qui en facilite l'utilisation. Comme son nom l'indique, il vise d'abord la simplicité et la lisibilité, sacrifiant parfois des fonctionnalités plus avancées offertes par d'autres pare-feux. Il s'agit du pare-feu par défaut sur Debian et ses dérivées (Ubuntu, par exemple).

---

## Installation

```bash
sudo apt install ufw
```

---

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

### Modification des politiques par défaut

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

---

### Autorisation de SSH

```bash
sudo ufw allow ssh
sudo ufw allow 22
```

### Autorisation pour un serveur Web

```bash
sudo ufw allow http
sudo ufw allow https
```

---

###  Ports et plages

```bash
sudo ufw allow 6000:6007/tcp
```

---

### IP et sous-réseaux

```bash
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24 to any port 22
```

---

### Interfaces réseau

```bash
sudo ufw allow in on ens33 to any port 80
```

---

### Activation et statut

```bash
sudo ufw enable
sudo ufw status verbose
```

---

### Suppression de règles

```bash
sudo ufw status numbered
sudo ufw delete 2
```
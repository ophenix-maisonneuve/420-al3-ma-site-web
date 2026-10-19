---
layout: default
title: "Fail2ban"
parent: "Durcissement d'un serveur"
nav_order: 2
published: true
---

# Fail2ban

Dans un contexte où la majorité des serveurs sont exposés en permanence à Internet, la sécurité des services réseau constitue un enjeu majeur pour les administrateurs système. Les tentatives d’intrusion automatisées, en particulier les attaques par force brute visant les mécanismes d’authentification, sont aujourd’hui constantes, massives et faciles d'accès même pour un amateur. Même lorsque des mécanismes de sécurité de base sont en place (mots de passe complexes, changement de ports, restrictions d’accès), l’absence de mécanisme de bannissement laisse la porte ouverte aux attaques automatisées.

Fail2ban est un outil de sécurité libre (*open source*) conçu pour analyser les fichiers de journalisation produits par le système et les services applicatifs. Son rôle n’est pas de remplacer les mécanismes d’authentification existants, mais de réduire drastiquement l’exposition aux attaques automatisées en bloquant les adresses IP ayant un comportement suspect.

Il s’appuie sur trois concepts fondamentaux : l’analyse de journaux (logs), la détection de motifs via des expressions régulières, et l’exécution d’actions correctives (pare-feu, notifications, scripts). Fail2ban intègre nativement des protections pour de nombreux services standards mais peut être étendu à des applications personnalisées.

---

### Attaques par force brute

Les attaques par force brute consistent à tester un grand nombre de combinaisons utilisateur/mot de passe jusqu’à obtenir un accès valide. Ces attaques sont aujourd’hui majoritairement automatisées et distribuées : plusieurs milliers d’adresses IP issues de réseaux compromis, comme des *botnets*, peuvent cibler un même service simultanément. Modifier un port par défaut n’est qu’un obstacle mineur contre ce type d’attaque et n’élimine en aucun cas le risque.

Sans mécanisme de bannissement, un attaquant peut effectuer un nombre astronomique de tentatives réparties dans le temps, sans jamais déclencher d’alerte. L’analyse manuelle des journaux devient rapidement irréaliste et peu efficace. Fail2ban apporte une réponse pragmatique : observer, compter, bannir, tout en évitant le plus possible de sévir contre un utilisateur légitime ayant fait une simple erreur.

---

### Fonctionnement de Fail2ban

Le fonctionnement de Fail2ban est basé sur une surveillance continue et en temps quasi réel des fichiers journaux. Chaque entrée de journal est comparée à un ensemble de motifs définis dans des filtres. Lorsqu’une correspondance est trouvée, Fail2ban incrémente un compteur associé à l’adresse IP source.

Ces tentatives sont analysées dans une fenêtre temporelle appelée `findtime`. Si le nombre d’échecs dépasse la limite autorisée (`maxretry`), Fail2ban déclenche une action, généralement un bannissement temporaire via le pare-feu.

---

### Terminologie

- **Filtre** : expressions régulières détectant des événements suspects
- **Action** : commandes exécutées lors d’un bannissement
- **Cellule (*jail*)** : association d'un filtre appliqué sur un journal (*log*) déclenchant une ou plusieurs action(s)
- **Client** : interface de contrôle
- **Serveur** : moteur d’application des règles

---

### Installation

```bash
sudo apt update
sudo apt install -y fail2ban
```

---

### Organisation des fichiers de configuration

Les personnalisations pour les cellules doivent être réalisées à l'un des deux endroits suivants :
-  `/etc/fail2ban/jail.local` ou
- en ajoutant un fichier par cellule personnalisée dans répertoire `/etc/fail2ban/jail.d` (méthode recommandée)

---

### Paramètres par défaut
La section [DEFAULT] indique les paramètres qui s'appliqueront par défaut à toutes les cellules, à moins d'être spécifiées particulièrement pour une ou plusieurs cellules.

```ini
[DEFAULT]
ignoreip = 127.0.0.1 192.168.1.0/24
findtime = 3600
bantime = 86400
maxretry = 3
```

---

### Configuration spécifique à une cellule

Il est possible d'écraser les paramètres par défaut pour une cellule en définissant une section correspondant à son nom. Par exemple, pour SSH :

```ini
[sshd]
enabled = true
port = ssh,2222
logpath = /var/log/auth.log
maxretry = 3
```

---

### Tests et validation

Pour recharger toutes les règles :
```bash
sudo fail2ban-client reload --all
```

Pour voir la liste des échecs et des adresses IP qui ont été bannies en réaction à ces échecs :
```bash
sudo fail2ban-client status <nom de la cellule | --all>
```

---

### Gestion des bannissements

Fail2ban permet de bannir ou débannir manuellement des adresses IP à des fins de test ou d’intervention.

Pour bannir une IP dans une cellule en particulier :
```bash
sudo fail2ban-client set <jail> banip <adresse_ip>
```

Pour débannir une IP d'une cellule en particulier :
```bash
sudo fail2ban-client set <jail> unbanip <adresse_ip>
```

Pour débannir une IP de toutes les cellules : 
```bash
sudo fail2ban-client unban <adresse_ip>
```
Pour débannir toutes les adresses IP (non recommandé sauf si vraiment nécessaire) :
```bash
sudo fail2ban-client unban <adresse_ip> --all
```

---

### Journalisation

Les journaux (*logs*) de Fail2ban se trouvent dans `/var/log/fail2ban.log`

---

## Liens utiles

- GitHub officielle de Fail2ban : [https://github.com/fail2ban/fail2ban](https://github.com/fail2ban/fail2ban)


---
layout: default
title: "Fail2ban - Règles personnalisées"
parent: "Fail2ban"
nav_order: 1
---


# Fail2ban : personnalisation

Les cellules fournies par défaut couvrent les services standards, mais les environnements de production, surtout lorsqu'ils servent des applications ou des sites personnalisés, nécessitent souvent l'ajout de nouvelles règles. La création de filtres personnalisés permet d’analyser des journaux spécifiques et d’appliquer des bannissements adaptées.

---

## Étapes de mise en place

### 1. Identifier et analyser les journaux

La première étape consiste à identifier le fichier de journalisation (*log*) contenant les événements à analyser, puis à analyser sa structure. Cette étape est cruciale afin de bien définir le filtre qui sera appliqué.


### 2. Création d’un filtre personnalisé

On crée ensuite un fichier contenant le filtre (généralement une expression réglulière) qui permettra d'identifier le comportement menant à un bannissement :

```bash
sudo nano /etc/fail2ban/filter.d/<nom du service>.conf
```

```ini
[Definition]
failregex = ^<HOST> .*Échec d'authentification.*$
ignoreregex =
```

{: .astuce}
> Le mot-clé `HOST` est obligatoire et permettra de sauvegarder le nom d'hôte ou l'IP à bannir en cas d'infraction.


### 3. Tester les filtres avec fail2ban-regex

```bash
sudo fail2ban-regex /var/log/monservice.log /etc/fail2ban/filter.d/<nom du service>.conf
```


### 4. Création d’une cellule personnalisée

On crée ensuite un fichier contenant les paramètres de la cellule (*jail*), notamment le nombre d'infractions requises et la durée du bannissement.

```bash
sudo nano /etc/fail2ban/jail.d/<nom du service>.conf
```

```ini
[nom du service]
enabled = true
filter = <nom du service>
logpath = /var/log/<nom du service>.log
port = 443
findtime = 600
maxretry = 5
bantime = 86400
```

### 5. Activation de la nouvelle cellule personnalisée

On active finalement la nouvelle cellule.

```bash
sudo fail2ban-client reload --all
```

ou

```bash
sudo systemctl <restart | reload> fail2ban
```

---

## Liens utiles

- Guide du développeur Fail2ban : [https://fail2ban.readthedocs.io/en/latest/](https://fail2ban.readthedocs.io/en/latest/)
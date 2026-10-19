---
layout: default
title: Apache
parent: Sécurisation d'un serveur web
nav_order: 2
---

# Apache

**Apache HTTP Server** (Apache httpd) est l’un des serveurs web les plus utilisés au monde. Il offre une grande flexibilité et permet de mettre en place facilement une configuration HTTPS basée sur des certificats X.509. Cette page présente les étapes essentielles pour activer SSL/TLS sur Apache, importer un certificat et sécuriser un site web.

---

## Activation du module SSL
Avant de pouvoir utiliser HTTPS, il faut activer le module SSL intégré à Apache :

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
```
Ce module permet à Apache de gérer les connexions HTTPS et les certificats X.509.

---

## Ajout du certificat et de la clé privée
Comme pour Nginx, Apache utilise les répertoires standards :

- `/etc/ssl/certs/` pour le certificat signé :
```bash
sudo cp certificat.crt /etc/ssl/certs/
```

- `/etc/ssl/private/` pour la clé privée :
```bash
sudo cp cle_privee.key /etc/ssl/private/
```

{: .warning}
> Les permissions de `/etc/ssl/private/` doivent empêcher tout accès non autorisé. Habituellement, les permissions sont `700` sur ce répertoire de façon à ne donner accès qu'à l'utilisateur `root`.

---

## Configuration du VirtualHost HTTPS
On crée un fichier de configuration pour le port sécurisé :

```bash
sudo nano /etc/apache2/sites-available/example.com.conf
```
Avec le contenu suivant :

```apache
<VirtualHost *:443>
    ServerName example.com
    DocumentRoot /var/www/example.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificat.crt
    SSLCertificateKeyFile /etc/ssl/private/cle_privee.key


    <Directory /var/www/example.com>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

# Redirection HTTP → HTTPS
<VirtualHost *:80>
    ServerName example.com
    Redirect / https://example.com/
</VirtualHost>

```

Cette configuration :
- active HTTPS sur le port 443
- associe le certificat et la clé privée
- définit le dossier racine du site web
- autorise l'accès via Apache.
- redirige le trafic HTTP vers HTTPS


## Ajout d'options de sécurité
On peut ensuite ajouter plusieurs directives pour améliorer la sécurité :

```apache
<VirtualHost *:443>
    ServerName example.com
    DocumentRoot /var/www/example.com

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificat.crt
    SSLCertificateKeyFile /etc/ssl/private/cle_privee.key

    <Directory /var/www/example.com>
        AllowOverride All
        Require all granted
    </Directory>

    # Protocoles SSL/TLS sécurisés
    SSLProtocol TLSv1.2 TLSv1.3

    # HSTS : force l’usage du HTTPS
    Header always set Strict-Transport-Security "max-age=31536000"

    # Anti-clickjacking
    Header always set X-Frame-Options "DENY"

    # Empêche l’analyse dangereuse du type MIME
    Header always set X-Content-Type-Options "nosniff"

    # Masque la version du serveur
    ServerSignature Off
    ServerTokens Prod

    # Empêche l’indexation des répertoires
    <Directory /var/www/exemple>
        Options -Indexes
        Require all granted
    </Directory>

    # Empêche l’accès aux fichiers sensibles (ajuster au besoin)
    <FilesMatch "^\.">
        Require all denied
    </FilesMatch>
</VirtualHost>

# Redirection HTTP → HTTPS
<VirtualHost *:80>
    ServerName exemple.com
    Redirect permanent / https://exemple.com/
</VirtualHost>
```

---

## Activation du site et redémarrage
On active ensuite le site :

```bash
sudo a2ensite example.com.conf
sudo systemctl reload apache2
```

Si tout est correct, Apache sert maintenant le site en HTTPS.

---

Apache permet une grande flexibilité pour déployer des sites sécurisés en HTTPS. L'ajout de plusieurs sites via l'instruction `VirtualHost` sur port 443 et la gestion de SSL/TLS intégrée de façon native en fait un excellent choix pour sécuriser des services web en production.

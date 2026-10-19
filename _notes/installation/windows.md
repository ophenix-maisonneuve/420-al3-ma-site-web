---
layout: default
title: "Windows 11"
nav_order: 2
parent: "Installation de l'environnement"
---

# Guide d'installation sous Windows 11

Ce guide vous explique comment installer **OpenJDK 25**, **Maven** et **Visual Studio Code** sous Windows 11.

---

## Installation de OpenJDK 25

### Méthode 1 : Installation manuelle (via le build Microsoft de OpenJDK)

1. Rendez-vous sur le site officiel : [https://learn.microsoft.com/en-us/java/openjdk/download](https://learn.microsoft.com/en-us/java/openjdk/download)
2. Téléchargez le JDK 25 pour Windows (format `.msi` ou `.zip`).
3. Installez-le en suivant les instructions de l'installateur.
4. Configurez les variables d'environnement :
   - Ajoutez le chemin du dossier `bin` du JDK à la variable `PATH`
   - Créez une variable `JAVA_HOME` pointant vers le dossier d'installation du JDK
5. Vérifiez l'installation :
```powershell
java -version
javac -version
```

### Méthode 2 : Installation via Chocolatey

1. Ouvrez PowerShell en mode administrateur
2. Installez Chocolatey si ce n'est pas déjà fait :
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iwr https://community.chocolatey.org/install.ps1 -UseBasicParsing | iex
```
3. Installez OpenJDK 25 :
```powershell
choco install openjdk --version=25
```
4. Vérifiez l'installation :
```powershell
java -version
```

---

## Installation de Maven

### Avec Chocolatey
```powershell
choco install maven
```

### Manuellement
1. Téléchargez Maven depuis : [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
2. Décompressez l'archive dans un dossier (ex. `C:\Program Files\Apache\maven`)
3. Ajoutez le chemin `bin` de Maven à la variable `PATH`
4. Vérifiez l'installation :
```powershell
mvn -version
```

---

## Installation de Visual Studio Code

1. Téléchargez VS Code depuis : [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Installez-le avec les options par défaut
3. (Optionnel) Installez les extensions Java :
   - Extension Pack for Java
   - Language Support for Java(TM) by Red Hat

---

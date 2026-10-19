---
layout: default
title: "macOS"
nav_order: 3
parent: "Installation de l'environnement"
---

# Installation sur macOS

## Java JDK 25
```bash
brew install openjdk@25
sudo ln -sfn /opt/homebrew/opt/openjdk@25/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-25.jdk
java -version
```

## Maven
```bash
brew install maven
mvn -version
```

## Visual Studio Code
```bash
brew install --cask visual-studio-code
```

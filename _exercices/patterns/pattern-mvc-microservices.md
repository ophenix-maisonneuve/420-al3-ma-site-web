---
layout: default
title: "Patrons architecturaux - MVC & microservices"
parent: "Patrons de conception"
nav_order: 1
has_toc: false
published: true
---
# Exercice : Recommandations de films (MVC + microservices)

## Objectifs

- Comprendre la différence de niveau entre l’architecture interne (MVC) et l’architecture système (microservices).
- Implémenter la logique métier d’un service de *catalogue* (MVC) et d’un service de *recommandations*.
- Expérimenter la collaboration entre services dans un modèle microservices
- Raisonner sur la résilience (comportement si l’autre service est indisponible).

## Contexte

Vous disposez de **deux projets Maven** utilisant Spring Boot pour gérer, entre autres, la communication via services REST. Les contrôleurs REST sont déjà fournis et n'ont pas à être modifiés afin de pouvoir se concentrer sur l'implémentation et l'architecture des services.

- `catalogue-service` : expose une liste de films avec `id`, `titre`, `genre`, `popularite`, par exemple :

  - `GET /films`
  - `GET /films?genre=Action`
  - `GET /films/{id}`

- `recommendations-service` : calcule un *Top N* par *genre* en appelant le service *catalogue* (client fourni), par exemple :

  - `GET /recommandations?genre=Action`

{: .highlight}
> Le but est de simuler une application évoluant en contexte d'entreprise: l'architecture par microservices permet qu'un deuxième service (MVC ou non) soit ajouté et géré de façon indépendante à un premier service existant.

## Étapes préparatoires

### 1. Clonez les dépôts de l’exercice

```bash
# Catalogue
git clone git@github.com:ophenix-420-930-ma-24636/catalogue-service.git
# ou
git clone https://github.com/ophenix-420-930-ma-24636/catalogue-service.git

# Recommandations
git clone git@github.com:ophenix-420-930-ma-24636/recommandations-service.git
# ou
git clone https://github.com/ophenix-420-930-ma-24636/recommandations-service.git
```

### 2. Lancez chaque projet Java

Dans chaque dépôt (un terminal par service) :

```bash
mvn clean package
mvn spring-boot:run
```

- **Catalogue** démarre par défaut sur **:8080**
- **Recommandations** démarre par défaut sur **:8081**

{: .astuce}
> Si besoin, vous pouvez surcharger l’URL du catalogue-service côté recommandations-service avec :
> `-Dspring-boot.run.jvmArguments="-DCATALOGUE_URL=http://localhost:8080"`

---

## 1. Service de catalogue (`catalogue-service`)
On désire implémenter un service ayant la responsabilité d'organiser un catalogue de films et pouvant, par exemple, servir de base à une plate-forme de diffusion (*streaming*).

### 1.1. Tentez d'interagir avec le projet `catalogue-service`
1. Lancez le projet à partir du répertoire de `catalogue-service`

```bash
mvn clean package spring-boot:run
```
2. À partir d'un navigateur ou d'un outil de requêtes HTTP comme *Postman*, effectuez la requête suivante :
```bash
GET http://localhost:8080/films
```
  - Qu'observez-vous ? Pourquoi ?


### 1.2. Étudiez la structure interne de **catalogue-service**

- Pour chaque rôle du MVC (modèle, vue et contrôleur), identifiez la ou les classes correspondantes:

  - **Modèle** : quelles classes représentent les entités métier, leur manipulation et leur persistance ?
  - **Vue** : quelles classes sont responsables de la préparation / présentation de la réponse au client ?
  - **Contrôleur** : Quelle classe reçoit les requêtes, les aiguille vers la logique métier et retourne la réponse correspondante ?

### 1.3. Étudiez les classes responsables de la persistance du service

- Où les films sont-ils sauvegardés ?
- Dans une véritable implémentation en production, quelle implémentation alternative pourrait-on envisager ?

### 1.4. Implémentez la logique de conversion dans `FilmMapper`
Cet objet sert à transformer un objet du modèle (`Film`) en une vue retournée à l'utilisateur (`FilmDto`).

- À quoi sert la distinction entre un objet `Film` et un objet `FilmDto` dans le contexte du MVC ?
- Pouvez-vous nommer des cas où il sera avantageux d'avoir bien découplé ces deux objets ?

### 1.5. Implémentez la logique métier du catalogue

On suppose que la première version de notre catalogue de films s'adresse à des enfants. Ainsi, on voudra s'assurer que, même s'il est possible que notre base de données contienne des films qui ne sont pas destinés aux enfants, notre implémentation de service empêche les enfants d'avoir accès aux films de genre `Horreur` et `Action` .

- Notre implémentation de `FilmService` est nommée `FilmServiceEnfants`.
- Dans `FilmServiceEnfants`, implémentez les méthodes requises par l'interface `FilmService`
  - Assurez-vous que toutes les méthodes excluent les films appartenant aux genres `Horreur` et `Action` des résultats
  - Assurez-vous que les id inexistants renvoient une **réponse vide** ou `null`
- Dans le contexte du MVC, pourquoi est-il important que cette logique d'exclusion appartienne aux implémentations de l'interface `FilmService` plutôt...
  - ... qu'à la classe `FilmController` ?
  - ... qu'à la classe `FilmMapper` ou `FilmDto` ?
- Vos méthodes doivent retourner des objets `FilmDto`. Quelle classe existante allez-vous utiliser pour cette opération ?


## 2. Service de recommandations (`recommandations-service`)

On désire maintenant ajouter à notre service de catalogue la possibilité de faire des recommandations basées sur la popularité de certains films.

### 2.1. Tentez d'interagir avec le projet `recommandations-service`
1. Lancez le projet à partir du répertoire de `recommandations-service`

```bash
mvn clean package spring-boot:run
```
2. À partir d'un navigateur ou d'un outil de requêtes HTTP comme *Postman*, effectuez la requête suivante :
```bash
GET http://localhost:8081/recommandations?genre=Action`
```
  - Qu'observez-vous ? Pourquoi ?

### 2.2. Comparez deux architectures possibles

Quels seraient les avantages et les inconvénients des solutions suivantes...

- Ajouter la fonctionnalité de recommandations directement dans le service de catalogue ?
  - Pensez, entre autres, à la complexité liée aux bases de données et à la communication entre les services...
- Ajouter la fonctionnalité de recommandations dans un service séparé (donc un projet complètement distinct) ?
  - Pensez, entre autres, à l'ajout de nouvelles fonctionnalités au fil du temps, à la facilité de déploiement et à la possibilité d'utiliser des langages ou technologies différentes pour différents services...

Laquelle des architectures ci-haut correspond à une architecture par microservices ? Pourquoi ?

### 2.3. Implémentez l’algorithme de recommandation

- Dans `DefaultRecommendationEngine`, implémentez `recommend(String genre)` :
  - Récupère la liste du Catalogue,
  - Trie par popularité décroissante,
  - Retourne le *Top 3* (les 3 films les plus populaires)
- Que se passe-t-il si le genre est vide ou non fourni ? Proposez un comportement raisonnable et justifiez.

### 2.4. Gérez l’indisponibilité du service de catalogue

Simulez une panne en éteignant le service de catalogue.

- Quel message renvoie le service de recommandations ?
- Quel comportement pourriez-vous implémenter pour gérer le cas où le catalogue est en panne de façon plus gracieuse ?
- Décrivez comment vous documenteriez ce comportement pour un autre client (contrat).

## Questions de réflexion

- **Niveaux** d’architecture : qu’est-ce qui, dans cet exercice, illustre clairement que **MVC** (interne) et **microservices** (système) ne se situent pas au même niveau ?
- **Frontières métier** : la séparation catalogue vs recommandations vous paraît-elle naturelle ? Pourquoi ? Aurait-on pu séparer autrement ?
- **Données par service** : que changerait une base partagée entre les services catalogue et recommandations? Quels seraient les risques et les avantages ?
- **Évolution** : si l’on remplaçait la façon dont service-recommandations calcule la popularité, par exemple en appelant un service tiers de statistiques, qu’est-ce que cela changerait dans le **contrat** et dans les **dépendances** ?

---
layout: default
title: Patrons de création
parent: Patrons de conception
nav_order: 2
has_toc: false
---

# Patrons de création

Les patrons de création se concentrent sur la manière dont les objets sont instanciés. Ils permettent de rendre la création d’objets plus flexible, mieux encapsulée et indépendante des types concrets.

## Cas d'utilisation
Ces patrons résolvent les difficultés liées à l’instanciation d’objets lorsque la création doit être abstraite, contrôlée ou découplée de l’utilisation. Ils évitent les constructeurs trop complexes ou les dépendances directes entre composants qui causent souvent un bris du principe d'inversion des dépendances (***DIP***).

## Patrons courants
- [Factory method](../notes/pattern-factory-method)
- [Abstract factory](../notes/pattern-abstract-factory)
- [Singleton](../notes/pattern-singleton)
- [Builder](../notes/pattern-builder)
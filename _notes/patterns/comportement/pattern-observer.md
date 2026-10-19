---
layout: default
title: Observer
parent: Patrons comportementaux
nav_order: 2
published: true
---

## Description
Observer définit une dépendance 1:N (un à plusieurs ou *one-to-many*) entre objets, de sorte que lorsque le sujet (l'objet observé) change d'état, tous ses observateurs sont notifiés et mis à jour automatiquement.

## Quand l'utiliser ?
- Lorsque plusieurs composants doivent réagir aux changements d’un sujet central.
- Pour réduire le couplage direct entre l’émetteur et ses consommateurs.

## Avantages
- Diffusion d’événements flexible et extensible.
- Réduction du couplage et meilleure testabilité.

## Inconvénients
- Ordre de notification non garanti.
- Gestion d’erreurs parfois complexe.
- Risque de fuites mémoire si les observateurs ne se désabonnent pas.

---

## Exemple

### Diagramme de classes
```mermaid
classDiagram
class Subject {
  <<interface>>
  +attach(o: Observer): void
  +detach(o: Observer): void
  +notifyObservers(): void
}
class Observer {
  <<interface>>
  +update(temperature: float): void
}
class WeatherStation {
    -observers: List~Observer~
}
class CurrentConditionsDisplay {
    -lastTemperature: float
    +getLastTemperature(): float
}
Subject <|.. WeatherStation
Observer <|.. CurrentConditionsDisplay
WeatherStation --> Observer : notifie
```

### Code Java
```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(float temperature);
}

interface Subject {
    void attach(Observer o);
    void detach(Observer o);
    void notifyObservers();
}

class WeatherStation implements Subject {
    private float temperature;
    private List<Observer> observers;

    public WeatherStation() {
        this.observers = new ArrayList<Observer>();
    }

    public float getTemperature() {
        return this.temperature;
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        this.notifyObservers();
    }

    @Override
    public void attach(Observer o) {
        this.observers.add(o);
    }

    @Override
    public void detach(Observer o) {
        this.observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer o : this.observers) {
            o.update(this.temperature);
        }
    }
}

class CurrentConditionsDisplay implements Observer {
    private float lastTemperature;

    public float getLastTemperature() {
        return this.lastTemperature;
    }

    @Override
    public void update(float temperature) {
        this.lastTemperature = temperature;
        System.out.println("Température actuelle: " + temperature);
    }
}

class Demo {
    public static void main(String[] args) {
        WeatherStation station = new WeatherStation();
        CurrentConditionsDisplay display = new CurrentConditionsDisplay();
        station.attach(display);
        station.setTemperature(21.5f);
        station.setTemperature(23.0f);
    }
}
```
---

## Liens utiles
- [https://refactoring.guru/design-patterns/observer](https://refactoring.guru/design-patterns/observer)
- [https://en.wikipedia.org/wiki/Observer_pattern](https://en.wikipedia.org/wiki/Observer_pattern)

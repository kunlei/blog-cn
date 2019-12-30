---
title: "Decorator Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Decorator Pattern** is one of the structural design patterns that are popularized by the original GoF book.
According to WikiPedia, it "allows behavior to be added to an individual object, dynamically, without affecting the behavior of other objects from the same class".


Let's see how we can use it in real life.
Suppose we go to a restaurant and order either chicken or pork and we can also order soup or noodle on top of the main menu.
We can define an abstract class for chicken and pork:
```java
public abstract class Food {
    protected String name;

    public String getName() {
        return name;
    }

    public abstract double getPrice();
}
```

And we define a decorator class for all addon items:
```java
public abstract class FoodDecorator extends Food {
    public abstract String getName();
}
```

Here are the implementations:
```java
public class Chicken extends Food {

    public Chicken() {
        name = "chicken";
    }

    @Override
    public double getPrice() {
        return 3.5;
    }
}
```


```java
public class Pork extends Food {

    public Pork() {
        name = "Pork";
    }

    @Override
    public double getPrice() {
        return 2.2;
    }
}
```


```java
public class Soup extends FoodDecorator {
    private Food food;

    public Soup(Food food) {
        this.food = food;
    }

    @Override
    public String getName() {
        return food.getName() + " + soup";
    }

    @Override
    public double getPrice() {
        return 1.1 + food.getPrice();
    }
}
```


```java
public class Noodle extends FoodDecorator {
    private Food food;

    public Noodle(Food food) {
        this.food = food;
    }

    @Override
    public String getName() {
        return food.getName() + " + noodle";
    }

    @Override
    public double getPrice() {
        return 0.8 + food.getPrice();
    }
}
```

Let's run a demo:
```java
public class DecoratorDemo {

    public static void main(String[] args) {
        Food chicken = new Chicken();
        Food pork = new Pork();

        Food chickenWithSoup = new Soup(chicken);
        System.out.println("menu name = " + chickenWithSoup.getName() + " : price = " + chickenWithSoup.getPrice());

        Food porkWithSoupAndNoodle = new Noodle(new Soup(pork));
        System.out.println("menu name = " + porkWithSoupAndNoodle.getName() + " : price = " + porkWithSoupAndNoodle.getPrice());

    }
}
```

and below are the results:
```java
menu name = chicken + soup : price = 4.6
menu name = Pork + soup + noodle : price = 4.1000000000000005
```
---
title: "Adapter Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Adapter Pattern** is one of the structural design patterns that connects two otherwise incompatible interfaces.
Adapters are often used in real life.
Suppose there is a car interface that can run and a vehicle interface that can drive.
An adapter is connect these two interfaces so that a car can act like a vehicle.
To see this, let's define two interfaces:

```java
public interface Car {
    void run();
}
```

```java
public interface Vehicle {
    void drive();
}
```

Then let's have some implementations:
```java
public class Nissan implements Car {
    @Override
    public void run() {
        System.out.println("A nissan running on the road!");
    }
}
```

```java
public class Toyota implements Car {
    @Override
    public void run() {
        System.out.println("A toyota running on the road!");
    }
}
```

```java
public class Bike implements Vehicle {
    @Override
    public void drive() {
        System.out.println("A bike is moving on the road!");
    }
}
```

```java
public class Sled implements Vehicle {
    @Override
    public void drive() {
        System.out.println("I see a sled!");
    }
}
```

Suppose now we want to treat a car as an vehicle, we just need to define an adapter class:
```java
public class CarAdapter implements Vehicle {
    private Car car;

    public CarAdapter(Car car) {
        this.car = car;
    }

    @Override
    public void drive() {
        car.run();
    }
}
```

Let's run a demo:
```java
public class AdapterDemo {

    public static void main(String[] args) {
        Car nissan = new Nissan();
        Car toyota = new Toyota();
        nissan.run();
        toyota.run();

        Vehicle bike = new Bike();
        Vehicle sled = new Bike();
        bike.drive();
        sled.drive();

        Vehicle nissanAdapter = new CarAdapter(nissan);
        nissanAdapter.drive();

        Vehicle toyotaAdapter = new CarAdapter(toyota);
        toyotaAdapter.drive();
    }
}
```

and here are the results:
```java
A nissan running on the road!
A toyota running on the road!
A bike is moving on the road!
A bike is moving on the road!
A nissan running on the road!
A toyota running on the road!
```
---
title: "Command Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Command Pattern** is one of the behavioral design patterns that consists of four essential components:
- Command: this is essentially an interface to be implemented by concrete classes. It decouples the invoker which is the user of various methods provided by receivers, so instead of a invoker directly calling methods provided by receivers, invokers call a common interface. An example of this pattern can be seen in the Runnable interface provided by JDK.
- Receiver: this is the concrete classes that define specific behaviors conforming to the method defined by the Command interface.

- Invoker: this is the user of the Command interface.

- Client: this control the initialization of receivers and invokers.


Here is an example of the Command interface:
```java
public interface Command {
    void execute();
}
```

Let's say you have a car which can speed up and slow down:
```java
public class Car {
    private int speed;

    public Car() {
        speed = 10;
        System.out.println("I am running at " + speed + " miles per hour!");
    }

    public void speedUp() {
        speed++;
        System.out.println("Speeding up... I am now running at " + speed + " miles per hour!");
    }

    public void slowDown() {
        speed--;
        System.out.println("Slowing down... I am now running at " + speed + " miles per hour!");
    }
}
```

Now instead of saving a reference to a Car object to directly controlling it to speed up or slow down, we can encapsulate the methods into individual objects:
```java
public class SpeedUpCommand implements Command {
    private Car car;

    public SpeedUpCommand(Car car) {
        this.car = car;
    }

    @Override
    public void execute() {
        car.speedUp();
    }
}
```

```java
public class SlowDownCommand implements Command {
    private Car car;

    public SlowDownCommand(Car car) {
        this.car = car;
    }

    @Override
    public void execute() {
        car.slowDown();
    }
}
```

The invoker here can be a driver:
```java
public class Driver {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void drive() {
        command.execute();
    }
}
```

Let's now run a demo:
```java
public class DrivingSim {

    public static void main(String[] args) {
        Car car = new Car();
        SpeedUpCommand speedUpCommand = new SpeedUpCommand(car);
        SlowDownCommand slowDownCommand = new SlowDownCommand(car);
        Driver driver = new Driver();

        driver.setCommand(speedUpCommand);
        driver.drive();

        driver.setCommand(slowDownCommand);
        driver.drive();
    }
}
```



{{ template "_internal/disqus.html" . }}



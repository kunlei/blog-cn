---
title: "Flyweight Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Proxy Pattern** is one of the structural design patterns that are popularized by the original GoF book.
In this pattern, a class can function as an interface to some other class.
To achieve this, the proxy class and the original class must implement the same interface.

In the example below, a computer implements the computing interface to carry out computation tasks.

```java
public interface Computing {
    void compute(int value);
}
```

The computer class just accepts a value and prints it.
```java
public class Computer implements Computing {

    @Override
    public void compute(int value) {
        System.out.println("value = " + value);
    }
}
```

The proxy defined below provides the same function with the computer class, and it provides additional operations:
```java
public class ComputerProxy implements Computing {
    private Computing computing;

    public ComputerProxy(Computing computing) {
        this.computing = computing;
    }

    @Override
    public void compute(int value) {
        if (value > 0) {
            computing.compute(value);
        } else {
            System.out.println("value = " + value + " is negative!");
        }
    }
}
```

Below is a demo:
```java
public class ProxyDemo {

    public static void main(String[] args) {
        Computing computer = new Computer();
        Computing proxy = new ComputerProxy(computer);

        proxy.compute(10);
        proxy.compute(-10);
    }
}
```

and the results:
```java
value = 10
value = -10 is negative!
```



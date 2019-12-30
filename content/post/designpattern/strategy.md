---
title: "Strategy Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Strategy Pattern**, also known as **policy pattern**, is one of the behavioral design patterns that promote software flexibility and reusability.
It features in allowing classes to select algorithms at runtime by encapsulating algorithms and making them interchangable. 

This pattern is better explained through an example.
Imagine we are tasked to model a group of kids within a preschool.
Everyone can sing and jump, but naturally the way they sing and jump vary kid by kid.
For simplicity, let's say a child can either sing loudly or quietly, and either jump high or low.
The strategy pattern defines separate interfaces for these behaviors:
```java
public interface Sing {
    void sing();
}
```

```java
public interface Jump {
    void jump();
}
```

Based on the possible ways of singing and jumping, various implementations can be possible:
```java
public class SingAloud implements Sing {
    @Override
    public void sing() {
        System.out.println("I am singing aloud!");
    }
}
```

```java
public class SingQuiet implements Sing {
    @Override
    public void sing() {
        System.out.println("I am singing quietly...");
    }
}
```

The same is true for jumping:
```java
public class JumpHigh implements Jump {
    @Override
    public void jump() {
        System.out.println("I can jump very high!");
    }
}
```

```java
public class JumpLow implements Jump {
    @Override
    public void jump() {
        System.out.println("I can only jump low...");
    }
}
```


Having defined the possible behaviors, we can go ahead and create a base class for children within the group:
```java
public abstract class Child {
    protected String name;
    protected Sing sing;
    protected Jump jump;

    public void setSing(Sing sing) {
        this.sing = sing;
    }

    public void setJump(Jump jump) {
        this.jump = jump;
    }

    public void sing() {
        sing.sing();
    }
    public void jump() {
        jump.jump();
    }
}
```

Note that this base class is decoupled fron concrete behaviors, it only stores a reference to the singing behavior and jumping behavior.
In other words, it lets its subclasses to decide which algorithm to use at runtime.
Let's look at some examples:
```java
public class XiaoMing extends Child {
    public XiaoMing() {
        name = "Xiao Ming";
        setSing(new SingAloud());
        setJump(new JumpHigh());
    }
}
```

```java
public class GouDan extends Child {
    public GouDan() {
        name = "Gou Dan";
        setSing(new SingQuiet());
        setJump(new JumpLow());
    }
}
```

```java
public class ErWa extends Child {
    public ErWa() {
        name = "Er Wa";
        setSing(new SingQuiet());
        setJump(new JumpHigh());
    }
}
```


Now let's watch children playing in the classroom.
Note that each child has his/her own default singing/jumping behavior at initialization.
The interesting part is that they can change their minds at runtime!

```java
public class ChildrenAtPlay {

    public static void main(String[] args) {
        // let's first define singing and jumping algorithms
        SingAloud singAloud = new SingAloud();
        SingQuiet singQuiet = new SingQuiet();
        JumpHigh jumpHigh = new JumpHigh();
        JumpLow jumpLow = new JumpLow();

        // here are some kids in the group
        XiaoMing xiaoMing = new XiaoMing();
        GouDan gouDan = new GouDan();
        ErWa erWa = new ErWa();

        // children at play
        xiaoMing.sing();
        xiaoMing.jump();
        gouDan.sing();
        gouDan.jump();
        erWa.sing();
        erWa.jump();

        // children change their minds all the time
        xiaoMing.setSing(singQuiet);
        xiaoMing.setJump(jumpLow);
        gouDan.setSing(singAloud);
        gouDan.setJump(jumpHigh);

        // children at play again
        System.out.println();
        xiaoMing.sing();
        xiaoMing.jump();
        gouDan.sing();
        gouDan.jump();
        erWa.sing();
        erWa.jump();
    }
}
```

Here are the simulation results:
```java
I am singing aloud!
I can jump very high!
I am singing quietly...
I can only jump low...
I am singing quietly...
I can jump very high!

I am singing quietly...
I can only jump low...
I am singing aloud!
I can jump very high!
I am singing quietly...
I can jump very high!
```


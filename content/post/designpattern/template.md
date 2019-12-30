---
title: "Template Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Template Pattern** is one of the behavioral patterns and it utilizes a base class to define a set of routines in fixed order.
Subclasses are free to instantiate any individual steps and the order the steps are executed is totally determined by the base class.
This is achieved by setting the ordering function to final.
Again, I will use my daughter's daily routine as an example.

Here, the base class defines what she does after school:
```java
public abstract class Routine {
    public final void perform() {
        playGames();
        readStories();
        freeTime();
    }

    protected abstract void playGames();
    protected abstract void readStories();
    protected abstract void freeTime();
}
```

The exact activities depend on her mood on each day:
```java
public class Monday extends Routine {
    @Override
    protected void playGames() {
        System.out.println("Monday: let's play Lego!");
    }

    @Override
    protected void readStories() {
        System.out.println("Monday: let's read 'Beauty and the Beast'");
    }

    @Override
    protected void freeTime() {
        System.out.println("Monday: wandering around...");
    }
}
```

```java
public class Tuesday extends Routine {
    @Override
    protected void playGames() {
        System.out.println("Tuesday: let's play pretend!");
    }

    @Override
    protected void readStories() {
        System.out.println("Tuesday: let's read 'Curious George'!");
    }

    @Override
    protected void freeTime() {
        System.out.println("Tuesday: hide and seek!");
    }
}
```

Here is a demo:
```java
public class TemplateDemo {

    public static void main(String[] args) {
        Routine monday = new Monday();
        Routine tuesday = new Tuesday();

        monday.perform();
        System.out.println();
        tuesday.perform();
    }
}
```

and the output:
```java
Monday: let's play Lego!
Monday: let's read 'Beauty and the Beast'
Monday: wandering around...

Tuesday: let's play pretend!
Tuesday: let's read 'Curious George'!
Tuesday: hide and seek!
```

Note: the template pattern looks similar to the policy pattern in that they both allow algorithms to vary in subclasses.
However, in the former, the subclasses define algorithms at compile-time through subclassing, while in the latter, subclasses can choose algorithms at run-time!

---
title: "Proxy Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Flyweight Pattern** is one of the structural design patterns that are popularized by the original GoF book.
This pattern is useful if one has to create a large number of objects and they share some invariants, so instead of duplicating the shared information and consuming large amount of memories, one can just keep one copy of each shared state.

In the example below, let's say there are five possible colors associated with a circle shape, however, the size of the circle can be different. 
Let's first define the color enum and circle class:

```java
public enum Color {
    YELLO,
    BLUE,
    GREEN,
    BLACK,
    WHITE
}
```

```java
public class Circle {
    private Color color;
    private int size;

    public Circle(Color color) {
        this.color = color;
        size = 0;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }

    @Override
    public String toString() {
        return "Circle{" +
                "color=" + color +
                ", size=" + size +
                '}';
    }
}
```

We also need a circle factory to return a circle when needed:
```java
public class CircleFactory {
    private static Map<Color, Circle> circles = new HashMap<>();

    public static Circle getCircle(Color color) {
        Circle circle;
        if (circles.containsKey(color)) {
            circle = circles.get(color);
        } else {
            circle = new Circle(color);
            circles.put(color, circle);
        }
        return circle;
    }
}
```

Below is a demo:
```java
public class FlyweightDemo {

    public static void main(String[] args) {
        for (int i = 0; i < 20; i++) {
            Circle circle = CircleFactory.getCircle(getRandColor());
            circle.setSize(i);
            System.out.println(circle.toString());
        }
    }

    public static Color getRandColor() {
        Random r = new Random();
        int randInt = r.nextInt(5);
        if (randInt == 0) {
            return Color.BLACK;
        } else if (randInt == 1) {
            return Color.BLUE;
        } else if (randInt == 2) {
            return Color.GREEN;
        } else if (randInt == 3) {
            return Color.WHITE;
        } else {
            return Color.YELLO;
        }
    }
}
```

and the results:
```java
Circle{color=GREEN, size=0}
Circle{color=BLUE, size=1}
Circle{color=YELLO, size=2}
Circle{color=BLACK, size=3}
Circle{color=YELLO, size=4}
Circle{color=GREEN, size=5}
Circle{color=BLACK, size=6}
Circle{color=GREEN, size=7}
Circle{color=BLUE, size=8}
Circle{color=GREEN, size=9}
Circle{color=YELLO, size=10}
Circle{color=BLUE, size=11}
Circle{color=BLUE, size=12}
Circle{color=GREEN, size=13}
Circle{color=BLUE, size=14}
Circle{color=YELLO, size=15}
Circle{color=BLUE, size=16}
Circle{color=BLACK, size=17}
Circle{color=GREEN, size=18}
Circle{color=BLUE, size=19}
```

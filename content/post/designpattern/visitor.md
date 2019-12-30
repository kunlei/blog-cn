---
title: "Visitor Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Visitor Pattern** is one of the behavioral patterns defined in the original GoF book.
It is used to decouple computations from the objects they operate on.
Say you have a group of classes that are related somehow and you have computations defined on top of them, instead of including the computation process inside the classes, visitor pattern lets you define separate computation classes that are unware of the internal representations of the classes they are going to work on.


This pattern needs to interfaces - Visitor and Visitable.
Below is an example to compute the average weight and height of a family (assume size of 4).
```java
public interface Visitor {
    void visit(Father father);
    void visit(Mother mother);
    void visit(Daughter daughter);
    void visit(Son son);
}
```

```java
public interface Visitable {
    void accept(Visitor visitor);
}
```

Here is the base class:
```java
public class Mother extends Member implements Visitable {

    public Mother(double height, double weight) {
        super(height, weight);
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

Here are the family members:
```java
public class Father extends Member implements Visitable {

    public Father(double height, double weight) {
        super(height, weight);
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

```java
public class Mother extends Member implements Visitable {

    public Mother(double height, double weight) {
        super(height, weight);
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

```java
public class Daughter extends Member implements Visitable {

    public Daughter(double height, double weight) {
        super(height, weight);
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

```java
public class Son extends Member implements Visitable {

    public Son(double height, double weight) {
        super(height, weight);
    }

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```


Now we can define a class responsible to compute family average height:
```java
public class AvgHeightVisitor implements Visitor {
    private double avgHeight;
    private int numMembers;

    public AvgHeightVisitor() {
        avgHeight = 0;
        numMembers = 0;
    }

    @Override
    public void visit(Father father) {
        avgHeight += father.getHeight();
        numMembers++;
    }

    @Override
    public void visit(Mother mother) {
        avgHeight += mother.getHeight();
        numMembers++;
    }

    @Override
    public void visit(Daughter daughter) {
        avgHeight += daughter.getHeight();
        numMembers++;
    }

    @Override
    public void visit(Son son) {
        avgHeight += son.getHeight();
        numMembers++;
    }

    public double getAvgHeight() {
        if (numMembers < 1) return 0;
        return avgHeight / numMembers;
    }
}
```

and weight:
```java
public class AvgWeightVisitor implements Visitor {
    private double avgWeight;
    private int numMembers;

    public AvgWeightVisitor() {
        avgWeight = 0;
        numMembers = 0;
    }

    @Override
    public void visit(Father father) {
        avgWeight += father.getWeight();
        numMembers++;
    }

    @Override
    public void visit(Mother mother) {
        avgWeight += mother.getWeight();
        numMembers++;
    }

    @Override
    public void visit(Daughter daughter) {
        avgWeight += daughter.getWeight();
        numMembers++;
    }

    @Override
    public void visit(Son son) {
        avgWeight += son.getWeight();
        numMembers++;
    }

    public double getAvgWeight() {
        if (numMembers < 1) return 0;
        return avgWeight / numMembers;
    }
}
```


Let's run a demo:
```java
public class VisitorDemo {

    public static void main(String[] args) {
        List<Visitable> family = new ArrayList<>();
        family.add(new Father(5.6, 150));
        family.add(new Mother(5.3, 120));
        family.add(new Daughter(2.5, 40));
        family.add(new Son(1.5, 20));

        // compute average height
        AvgHeightVisitor avgHeightVisitor = new AvgHeightVisitor();
        for (Visitable member : family) {
            member.accept(avgHeightVisitor);
        }
        System.out.println("Average height = " + avgHeightVisitor.getAvgHeight());

        // compute average weight
        AvgWeightVisitor avgWeightVisitor = new AvgWeightVisitor();
        for (Visitable member : family) {
            member.accept(avgWeightVisitor);
        }
        System.out.println("Average weight = " + avgWeightVisitor.getAvgWeight());
    }
}
```

and here are the results:
```java
Average height = 3.7249999999999996
Average weight = 82.5
```




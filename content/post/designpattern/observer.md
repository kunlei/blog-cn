---
title: "Observer Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Observer Pattern** is a behavioral pattern suitable for decoupling objects with one-to-many relations.
There are two components in this pattern:

- Subject: this is the object whose state is subject to changes and will affect many following objects. 
- Observer: this is the object which will updates its status based on the status of the subject.


An example is the information updating system used in my daughter's school.
Parents get udpates from the class teacher constantly. 



```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

```java
public interface Observer {
    void update(String status);
}
```


```java
/**
 * Subject implementation
 * @author Kunlei Lian
 */
public class Teacher implements Subject {
    private String classStatus;
    private List<Observer> observers;

    public Teacher() {
        observers = new ArrayList<>();
    }

    public void updateClassStatus(String time) {
        switch (time) {
            case "morning":
                classStatus = "Kids are playing in classroom!";
                break;
            case "noon":
                classStatus = "Kids are having nap time!";
                break;
            case "afternoon":
                classStatus = "Kids are having fun on playground!";
                break;
            default:
                classStatus = "Kids are idle....";
                break;
        }
        notifyObservers();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer o : observers) {
            o.update(classStatus);
        }
    }
}
```


```java
/**
 * Observer implementation
 * @author Kunlei Lian
 */
public class Parent implements Observer {
    private String name;
    private Subject teacher;

    public Parent(String name, Subject teacher) {
        this.name = name;
        this.teacher = teacher;
        this.teacher.registerObserver(this);
    }

    @Override
    public void update(String status) {
        System.out.println(name + " got update from Teacher: " + status);
    }

    public void exitProgram() {
        teacher.removeObserver(this);
    }
}
```

Let's run a demo:
```java
/**
 * Preschool information updating system demo
 * @author Kunlei Lian
 */
public class ObserverDemo {

    public static void main(String[] args) {
        // a subject with two observers
        Teacher teacher = new Teacher();
        Parent parenta = new Parent("Parent AA", teacher);
        Parent parentb = new Parent("Parent BB", teacher);

        // subject broadcasts events to observers
        System.out.println();
        teacher.updateClassStatus("morning");
        teacher.updateClassStatus("noon");
        teacher.updateClassStatus("afternoon");

        // new observer
        System.out.println();
        Parent parentc = new Parent("Parent CC", teacher);
        teacher.updateClassStatus("morning");

        // existing observer stop observing
        System.out.println();
        parentb.exitProgram();
        teacher.updateClassStatus("noon");
    }
}
```

Here are the outputs:
```java
Parent AA got update from Teacher: Kids are playing in classroom!
Parent BB got update from Teacher: Kids are playing in classroom!
Parent AA got update from Teacher: Kids are having nap time!
Parent BB got update from Teacher: Kids are having nap time!
Parent AA got update from Teacher: Kids are having fun on playground!
Parent BB got update from Teacher: Kids are having fun on playground!

Parent AA got update from Teacher: Kids are playing in classroom!
Parent BB got update from Teacher: Kids are playing in classroom!
Parent CC got update from Teacher: Kids are playing in classroom!

Parent AA got update from Teacher: Kids are having nap time!
Parent CC got update from Teacher: Kids are having nap time!
```


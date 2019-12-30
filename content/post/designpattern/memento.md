---
title: "Memento Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Memento Pattern** is one of the behavioral design patterns defined in the original GoF book.
It is useful when we need to save the state of an object and restore it to a saved state at a later time.
There are three components in this pattern:

- **Originator**: this is the class whose state needs to be saved and restored.
- **Memento**: this is the class used to capture the state of the originator.
- **Caretaker**: this is the class taking care of the saved mementos.


One example I can think of is the driver seat in my car, which provides the capability to set two positions.
Since I am physically bigger, I'd like a position where the seat is far from the steering wheel.
My wife, on the other hand, prefers a shorter distance between them.
Once set, the seat will adjust to preset position according to the position identifier. 
Our car installs a switch which allows adjustment of driver seat, a pretty nice feature!


To implement this feature, let's define a DriverSeat class, which is the originator of the pattern described above. 
```java
/**
 * Originator
 * @author Kunlei Lian
 */
public class DriverSeat {
    private String position;

    public DriverSeat() {
    }

    public void setPosition(String position) {
        this.position = position;
    }

    /**
     * Create position memento based on current position
     * @return new memento object
     */
    public Memento createMemento() {
        return new Memento(position);
    }

    /**
     * Restore to given position saved in memento
     * @param memento input memento
     */
    public void setMemento(Memento memento) {
        this.position = memento.getPosition();
    }

    @Override
    public String toString() {
        return "DriverSeat{" +
                "position='" + position + '\'' +
                '}';
    }
}
```

We also need a class to save a snapshot of the DriverSeat class per user request:
```java
/**
 * Memento
 * @author Kunlei Lian
 */
public class Memento {
    private String position;

    public Memento(String position) {
        this.position = position;
    }

    public String getPosition() {
        return position;
    }
}
```


The class below simulates part of the car's internal system to save user-defined seat positions, which also includes a demo of the pattern.
```java
/**
 * Caretaker
 * @author Kunlei Lian
 */
public class Caretaker {
    private Map<Integer, Memento> positions;

    public Caretaker() {
        positions = new HashMap<>();
    }

    public void savePosition(int posId, Memento memento) {
        positions.put(posId, memento);
        System.out.println("DriverSeat position = " + posId + " state = \"" + memento.getPosition() + "\" saved!");
    }

    public Memento getPosition(int posId) {
        return positions.get(posId);
    }

    public static void main(String[] args) {
        DriverSeat driverSeat = new DriverSeat();
        Caretaker caretaker = new Caretaker();

        // create first seat memento
        driverSeat.setPosition("User Position 1");
        caretaker.savePosition(1, driverSeat.createMemento());

        // create second seat memento
        driverSeat.setPosition("User Position 2");
        caretaker.savePosition(2, driverSeat.createMemento());

        // restore to designated user position
        driverSeat.setMemento(caretaker.getPosition(1));
        System.out.println(driverSeat.toString());

        driverSeat.setMemento(caretaker.getPosition(2));
        System.out.println(driverSeat.toString());
    }
}
```

Simulation results:
```java
DriverSeat position = 1 state = "User Position 1" saved!
DriverSeat position = 2 state = "User Position 2" saved!
DriverSeat{position='User Position 1'}
DriverSeat{position='User Position 2'}
```

I think the idea of this pattern is to allow the caretaker saving previous states of the originator without knowing its internal representation.
Also, the originator may just save part of its state.
A good example of this pattern used in JDK can be found in classes implementing the **Serializable** interface.
Object serialization is the process of converting an object's state to a series of bytes which can be saved to persistent storage and later used to restore the original object.



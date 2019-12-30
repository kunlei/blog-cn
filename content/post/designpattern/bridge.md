---
title: "Bridge Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Bridge Pattern** is one of the structural design patterns and it aims to separate abstractions from implementations by allowing them to be developed independently.
This may better be explained by an example.
Think of a company with two types of employees: onsite and remote. 
Naturally, the way they attend meetings differ based on the location they work.
An onsite employee can join meetings either online or in person, while a remote employee can only call in meeting online.


Let's define the meeting interface as below.
```java
public interface Meeting {
    void meet();
}
```

and two meeting behaviors:
```java
public class MeetingInPerson implements Meeting {
    @Override
    public void meet() {
        System.out.println("I am attending meetings in person!");
    }
}
```

```java
public class MeetingOnline implements Meeting {
    @Override
    public void meet() {
        System.out.println("I am attending meetings by calling in online!");
    }
}
```

Now we define the base class for employee which constains a reference to the meeting interface:
```java
public abstract class Employee {
    protected Meeting meeting;

    public Employee(Meeting meeting) {
        this.meeting = meeting;
    }

    public abstract void meeting();
}
```
and we have two types of employees:
```java
public class OnsiteEmployee extends Employee {

    public OnsiteEmployee(Meeting meeting) {
        super(meeting);
    }

    @Override
    public void meeting() {
        System.out.println("Hello, I am working onsite!");
        meeting.meet();
    }
}
```

```java
public class RemoteEmployee extends Employee {

    public RemoteEmployee(Meeting meeting) {
        super(meeting);
    }

    @Override
    public void meeting() {
        System.out.println("Hello, I am working remotely!");
        meeting.meet();
    }
}
```

Let the compnay run:
```java
public class BridgeDemo {

    public static void main(String[] args) {
        Meeting meetingOnline = new MeetingOnline();
        Meeting meetingInPerson = new MeetingInPerson();

        Employee onsiteEmployee = new OnsiteEmployee(meetingInPerson);
        Employee remoteEmployee = new RemoteEmployee(meetingOnline);

        onsiteEmployee.meeting();
        remoteEmployee.meeting();
    }
}
```

Here is the output:
```java
Hello, I am working onsite!
I am attending meetings in person!
Hello, I am working remotely!
I am attending meetings by calling in online!
```
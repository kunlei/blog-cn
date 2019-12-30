---
title: "Composite Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Composite Pattern** is one of the structural design patterns and it has three main components:

- Component: this is an interface to be implemented by either a leaf or a composite
- Leaf: an end node of a tree object structure
- Composite: a composite has multiple leaves

An example of composite pattern can be found in companies, which normally have two types of employees, individual contributors and managers.
We can therefore represent them in a tree structure and the common component interface allow uniform treatment of these two types of workers.
Let's define the component interface as below.
```java
public interface Employee {
    void work();
}
```

and a leaf and composite:
```java
public class IndividualContributor implements Employee {
    private String name;

    public IndividualContributor(String name) {
        this.name = name;
    }

    @Override
    public void work() {
        System.out.println("My name is " + name + " and I am working as individual contributor!");
    }
}
```

```java
public class Manager implements Employee {
    private String name;
    private List<Employee> team;

    public Manager(String name) {
        this.name = name;
        team = new ArrayList<>();
    }

    public void addMember(Employee e) {
        team.add(e);
    }

    @Override
    public void work() {
        System.out.println("My name is " + name + " and I am leading a team below:");
        for (Employee e : team) {
            e.work();
        }
    }
}
```


Let's run a demo:
```java
public class CompositeDemo {

    public static void main(String[] args) {
        IndividualContributor john = new IndividualContributor("John");
        IndividualContributor mike = new IndividualContributor("Mike");
        IndividualContributor kai = new IndividualContributor("Kai");

        Manager manager = new Manager("Joe");
        manager.addMember(john);
        manager.addMember(mike);

        List<Employee> org = new ArrayList<>();
        org.add(kai);
        org.add(manager);
        for (Employee e : org) {
            e.work();
        }
    }
}
```

Here is the output:
```java
My name is Kai and I am working as individual contributor!
My name is Joe and I am leading a team below:
My name is John and I am working as individual contributor!
My name is Mike and I am working as individual contributor!
```
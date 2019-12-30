---
title: Deep Dive into CPLEX Java API (1)
draft: false
date: 2019-10-11
tags: ["cplex", "Java"]
---

I have been using CPLEX for a while but haven't paid much attention to how its various components are woven together to enable users building and solving optimization models.
It is time to dive deeper into CPLEX's design principle and see how it works internally.
This is an attempt to improve my knowledge about CPLEX, and the random notes below are just my thinking aloud.

### IloAddable
**IloAddable** is the base interface implemented by most classes in the package.
It marks one CPLEX class as a optimization component and provides functions to get/set its name.
This interface might look like below internally:
```java
public interface IloAddable {
    String getName();
    void setName(String name);
}
```

One implication of this design is that we can give each model component a name and method to retrieve it.


### IloModel
**IloModel** is another base interface specifying functions that optimization model classes must implement.
It is interesting to note that this interface itself extends IloAddable, which allows us assigning names to models.
Naturally, a model can accomplish little without components like variables, constraints and objective function(s) in it.
That's probably why this base modeling interface encompasses methods to add and remove instances of component classes.
Since the model has to allow either constraints or objectives being added to it, parameters of these add/remove methods are set as IloAddable objects.
Technically, we can add a modeling object into another modeling object.

```java
public interface IloModel extends IloAddable {
    IloAddable add(IloAddable object);
    IloAddable remove(IloAddable object);
    java.util.Iterator iterator();
}
```

It's worth noting that modeling components can be added individually or, more efficiently, in arrays.


### IloModeler
**IloModeler** builds on top of IloModel to add methods for creating variables, constraints and objectives, and many ancillary methods.
One may choose to include the various components into a model immediately after their constructions or do that in a later stage.
The resulting model represents a generic model that can be solved by either CPLEX or CP Optimizer.

```java
public interface IloModeler extends IloModel {
    // methods to create constraints and add to model
    Constraint addEq(...);
    Constraint addGe(...);
    Constraint addLe(...);
    Constraint addRange(...);

    // methods to create constraints without adding them to model
    Constraint eq(...);
    Constraint ge(...);
    Constraint le(...);
    Constraint range(...);

    // methods to create objectives and add to model
    Objective addMaximize(...);
    Objective addMinimize(...);
    Objective addObjective(...);

    // methods to create objectives without adding them to model
    Objective maximize(...);
    Objective minimize(...);
    Objective objective(...);

    // methods to create variables and add to model
    Variable boolVar(...);
    Variable intVar(...);
    Variable numVar(...);

    // ancillary methods
    IloAdd and(...);
    IloOr or(...);
    Expression not(...);

    Expression constant(...);
    Expression diff(...);
    Expression intExpr(...);
    Expression linearIntExpr(...);
    Expression numExpr(...);
    Expression linearNumExpr(...);
    Expression max(...);
    Expression min(...);
    Expression negative(...);
    Expression prod(...);
    Expression scalProd(...);
    Expression square(...);
    Expression sum(...);
}
```

<!-- ![test image](/img/logo.png) -->



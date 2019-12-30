---
title: Cplex Java Concert Technology - Variables
authorbox: true
sidebar: false
draft: false
date: 2019-03-04
categories:
    - "Cplex"
tags:
    - "cplex"
    - "Java"
---

Cplex provides three types of decision variables, which are defined by class *IloNumVarType*. 
There are two key information about a variable: **bound** and **type**, the former can be changed after a variable is created, but the latter remains the same throughout the lifetime of the object.
Also, a variable bound consists of lower bound and upper bound, defined by class *IloNumVarBoundType*.

Although variables are key components of a mathematical model, Cplex does not provide function to explicitly add variables to a model, instead, variables are added implicitly. 
For example, when a constraint is added to a model, all the variables included in that constraint are added to the model as well.


### Variables
Concert Technology defines two interfaces for decision variables: **IloNumVar** and **IloIntVar**.
IloNumVar can represent all three variable types and the type information can be queried by *getType()*.
The interface itself extends IloNumExpr and provides the following functions:

+ double getLB()
+ void setLB(double lb)
+ double getUB()
+ void setUB(double ub)
+ IloNumVarType getType()

IloIntVar can represent either an Int variable or a Bool variable. 
The interface extends IloNumVar and provides the following extra functions:

+ int getMin()
+ void setMin(int min)
+ int getMax()
+ void setMax(int max)

### Variable Instantiation
In Cplex, classes implementing the variable interfaces are defined by classes implementing modeling interfaces, which include *IloModeler* and *IloMPModeler*.
I'll discuss the differences between these two interfaces in separate post.
If you are familiar with design patterns, these two modeling interfaces act as factories that can produce objects of other classes.
Let's look individually how each variable is created.

#### Bool variable
*IloModeler* provides the following functions to create Bool variables:

+ IloIntVar boolVar()
+ IloIntVar boolVar(String name)
+ IloIntVar[] boolVarArray(int n)
+ IloIntVar[] boolVarArray(int n, String[] name)

*IloMPModeler* provides additional ways to create Bool variables:

+ IloIntVar boolVar(IloColumn column)
+ IloIntVar boolVar(IloColumn column, String name)
+ IloIntVar[] boolVarArray(IloColumnArray cols)
+ IloIntVar[] boolVarArray(IloColumnArray cols, String[] name)

#### Int variable
*IloModeler* provides the following functions to create Int variables:

+ IloIntVar intVar(int min, int max)
+ IloIntVar intVar(int min, int max, String name)
+ IloIntVar[] intVarArray(int n, int[] min, int[] max)
+ IloIntVar[] intVarArray(int n, int[] min, int[] max, String[] name)
+ IloIntVar[] intVarArray(int n, int min, int max)
+ IloIntVar[] intVarArray(int n, int min, int max, String[] name)


*IloMPModeler* provides the following functions to create Int variables:

+ IloIntVar boolVar(IloColumn column)
+ IloIntVar boolVar(IloColumn column, String name)
+ IloIntVar[] boolVarArray(IloColumnArray cols)
+ IloIntVar[] boolVarArray(IloColumnArray cols, String[] name)

#### Float variable
*IloModeler* provides the following functions to create Float variables:

+ IloNumVar numVar(double lb, double ub)
+ IloNumVar numVar(double lb, double ub, String name)
+ IloNumVar numVar(double lb, double ub, IloNumVarType type)
+ IloNumVar numVar(double lb, double ub, IloNumVarType type, String name)
+ IloNumVar[] numVarArray(int n, double[] lb, double[] ub)
+ IloNumVar[] numVarArray(int n, double[] lb, double[] ub, String[] name)
+ IloNumVar[] numVarArray(int n, double[] lb, double[] ub, IloNumVarType[] type)
+ IloNumVar[] numVarArray(int n, double[] lb, double[] ub, IloNumVarType[] type, String[] name)
+ IloNumVar[] numVarArray(int n, double lb, double ub)
+ IloNumVar[] numVarArray(int n, double lb, double ub, String[] name)
+ IloNumVar[] numVarArray(int n, double lb, double ub, IloNumVarType type)
+ IloNumVar[] numVarArray(int n, double lb, double ub, IloNumVarType type, String[] name)

*IloMPModeler* provides the following functions to create Float variables:

+ IloNumVar numVar(IloColumn column, double lb, double ub)
+ IloNumVar numVar(IloColumn column, double lb, double ub, String name)
+ IloNumVar numVar(IloColumn column, double lb, double ub, IloNumVarType type)
+ IloNumVar numVar(IloColumn column, double lb, double ub, IloNumVarType type, String name)
+ IloNumVar[] numVarArray(IloColumnArray cols, double[] lb, double[] ub)
+ IloNumVar[] numVarArray(IloColumnArray cols, double[] lb, double[] ub, String[] name)
+ IloNumVar[] numVarArray(IloColumnArray cols, double[] lb, double[] ub, IloNumVarType[] type)
+ IloNumVar[] numVarArray(IloColumnArray cols, double[] lb, double[] ub, IloNumVarType[] type, String[] name)
+ IloNumVar[] numVarArray(IloColumnArray cols, double lb, double ub)
+ IloNumVar[] numVarArray(IloColumnArray cols, double lb, double ub, String[] name)
+ IloNumVar[] numVarArray(IloColumnArray cols, double lb, double ub, IloNumVarType type)
+ IloNumVar[] numVarArray(IloColumnArray cols, double lb, double ub, IloNumVarType type, String[] name)


### IloNumVarType Implementation

Cplex Concert defined three variable types, namely, **Float**, **Int** and **Bool**, in class [**IloNumVarType**](https://www.ibm.com/support/knowledgecenter/SSSA5P_12.8.0/ilog.odms.cplex.help/refjavacplex/html/ilog/concert/IloNumVarType.html).
This post examines at how this class is defined, and how objects of this class are created using concert technology.



Initially I thought **IloNumVarType** was implemented using as enumeration, something like below:

```java
public enum VarType {
    AAA(0, "AAA");

    private String name;
    private int value;

    VarType(int value, String name) {
        this.name = name;
        this.value = value;
    }

    public String getName() {
        return name;
    }

    public int getValue() {
        return value;
    }
}
```

Surprisingly, it was actually implemented using abstract class with static final fields.
```java
public abstract class VarType {
    public static final VarType AAA = new _AAA();

    public VarType() {
    }

    public abstract int getTypeValue();

    private static class _AAA extends VarType {
        private  _AAA() {
        }

        public String toString() {
            return "AAA";
        }

        @Override
        public int getTypeValue() {
            return 0;
        }
    }
}
```

As I think about it now, it seems the intention is to only expose interface, instead of implementation, to users, just like other interfaces defined in the concert package.
Two other enumeration-like classes also use this strategy:

+ [**IloNumVarBoundType**](https://www.ibm.com/support/knowledgecenter/SSSA5P_12.8.0/ilog.odms.cplex.help/refjavacplex/html/ilog/concert/IloNumVarBoundType.html)
+ [**IloObjectiveSense**](https://www.ibm.com/support/knowledgecenter/SSSA5P_12.8.0/ilog.odms.cplex.help/refjavacplex/html/ilog/concert/IloObjectiveSense.html)



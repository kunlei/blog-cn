---
title: Build Optimization Models by Row using CPLEX Java API
draft: false
date: 2019-10-15
tags: ["cplex", "Java"]
---

The CPLEX Java API separates model building from solving algorithms by defining various interfaces in the Concert package.
There are two ways in constructing a mathematical model regarding how constraints are added to the model:

* by row
* by column

## An illustrative example
This post looks at how to build models by row.
Assuming we have a small mathematical formulation as below:

$$
\begin{align}
\text{max.} \quad & 20 x_1 + 15 x_2 \newline
\text{s.t.} \quad & x_1 + 2x_2 \geq 20 \newline
\quad & 2x_1 + 3x_2 \leq 200 \newline
\quad & 0 \leq x_1 \leq 50, x_2 \geq 10
\end{align}
$$

The code snippets in following sections show three options in creating a CPLEX model in (slightly) different ways.
The point I am trying to make is that the Concert package defines a set of interfaces, including IloModeler and IloMPModeler, that specify methods to be implemented by model-building classes.
Note that IloModeler and IloMPModeler themselves don't define the interface of the solving methods.
In other words, they only do what their names imply, that is, building CPLEX models.

## Typical model building workflow
The code below illustrates a typical model-building workflow.
An IloCplex object is first created and passed to various model building methods.
The model is then solved by the *solve()* method defined within IloCplex.
The three methods in the switch block may appear similar at first glance in that they all take an IloCplex object as input argument
However, the underlying mechanism is vastly different, which we will examine shortly.

```java
public static void main(String[] args) {
  try {
    IloCplex cplex = new IloCplex();

    int method = 1;
    switch (method) {
      case 1:
        // build models using IloCplex
        buildModelWithIloCplex(cplex);
        break;
      case 2:
        // build models using IloModeler
        buildModelWithIloModeler(cplex);
        break;
      case 3:
      default:
        // build models using IloMPModeler
        buildModelWithIloMPModeler(cplex);
        break;
    }

    if (cplex.solve()) {
      System.out.println("objective value = " + cplex.getObjValue());
    }
  } catch (IloException e) {
    e.printStackTrace();
  }
}
```

## Building models using IloCplex
The following code is probably the most widely used way of building mathematical models in CPLEX.
An IloCplex object, named as cplex, is passed as input argument into the method and it does all the heavy lifting of model building.
The row-wise model building starts with creating decision variables, which are usually of type IloNumVar, an interface defined in the Concert package.
Note that the code shows a concise, and recommended, way to create variables in one line, instead of creating variable individually.
This is done by invoking the *numVarArray* method, which takes a couple of parameters:

* the number of decision variables to be created
* the array of lower bounds imposed on the variables
* the array of upper bounds defined on the variables
* the (optional) variables names in an array

With decision variables defined and ready for use, the next step is to specify a objective function.
I'll explore alternative ways to create objectives in CPLEX in future posts, the code below shows just one way.
An objective function consists of two components: an expression and an objective sense.
The former is created using a helper function named *scalProd*, which takes two input parameters, an array of variables and their corresponding coefficients. 
The *addMaximize* method adds a maximization objective to the model, and returns an reference of IloObjective.

Next comes the fun part, and it is the reason why we call this model building approach 'by row'.
Note that *IloRange* is an interface defined in Concert package to represent a constraint.
Since the constraints in our mathematical formulation have only lower bound or upper bound, not both, we uses the convenient methods *addGe()* and *addLe()* to define *and* add constraints into the model.
The methods are intuitive to use, we just need to give them an expression, a right hand size and possibly a name.

One caveat is that the *ge()* and *le()* are also available, but they serve to only build constraints, without adding the built constraints into the model.
If you decide to use them, remember to add the constraints to the model specifically, otherwise, the model won't do the magic for you.

```java
/**
  * using IloCplex instance to create model directly
  * @param cplex instance of IloCplex
  * @throws IloException
  */
private static void buildModelWithIloCplex(IloCplex cplex) throws IloException {
  // create decision variables
  double[] lb = {0.0, 10.0};
  double[] ub = {50.0, Double.MAX_VALUE};
  String[] varNames = {"x1", "x2"};
  IloNumVar[] var = cplex.numVarArray(2, lb, ub, varNames);

  // define objective function
  double[] objCoefs = {20.0, 15.0};
  IloObjective obj = cplex.addMaximize(cplex.scalProd(objCoefs, var));

  // construct constraints
  IloRange[] cons = new IloRange[2];
  cons[0] = cplex.addGe(cplex.sum(cplex.prod(1.0, var[0]), cplex.prod(2.0, var[1])), 20.0, "c1");
  cons[1] = cplex.addLe(cplex.sum(cplex.prod(2.0, var[0]), cplex.prod(3.0, var[1])), 200.0, "c2");
}
```


## Building models using IloModeler
The method below builds the same model in CPLEX with (almost) the same method body.
The only difference is that the input parameter is now an instance of a class implementing the IloModeler interface.
As discussed in other post, IloModeler defines various methods to build a model in CPLEX and it does not restrict you of which solving method to use later.
Not surprisingly, IloCplex is a class implementing the IloModeler interface.

```java
/**
  * using IloModeler to create model
  * @param modeler interface
  * @throws IloException
  */
private static void buildModelWithIloModeler(IloModeler modeler) throws IloException {
  // create decision variables
  double[] lb = {0.0, 10.0};
  double[] ub = {50.0, Double.MAX_VALUE};
  String[] varNames = {"x1", "x2"};
  IloNumVar[] var = modeler.numVarArray(2, lb, ub, varNames);

  // define objective function
  double[] objCoefs = {20.0, 15.0};
  IloObjective obj = modeler.addMaximize(modeler.scalProd(objCoefs, var));

  // construct constraints
  IloRange[] cons = new IloRange[2];
  cons[0] = modeler.addGe(modeler.sum(modeler.prod(1.0, var[0]), modeler.prod(2.0, var[1])), 20.0, "c1");
  cons[1] = modeler.addLe(modeler.sum(modeler.prod(2.0, var[0]), modeler.prod(3.0, var[1])), 200.0, "c2");
}
```

## Building models using IloMPModeler
The method below is similar to the one given in the previous section.
IloMPModeler is another interface implemented by IloCplex that specifies various model-building methods.
Note that IloMPModeler extends IloModeler and adds additional facilities to enable model building by column, which we will explore in future posts.

```java
/**
  * using IloMPModeler to build model
  * @param mpModeler interface
  * @throws IloException
  */
private static void buildModelWithIloMPModeler(IloMPModeler mpModeler) throws IloException {
  // create decision variables
  double[] lb = {0.0, 10.0};
  double[] ub = {50.0, Double.MAX_VALUE};
  String[] varNames = {"x1", "x2"};
  IloNumVar[] var = mpModeler.numVarArray(2, lb, ub, varNames);

  // define objective function
  double[] objCoefs = {20.0, 15.0};
  IloObjective obj = mpModeler.addMaximize(mpModeler.scalProd(objCoefs, var));

  // construct constraints
  IloRange[] cons = new IloRange[2];
  cons[0] = mpModeler.addGe(mpModeler.sum(mpModeler.prod(1.0, var[0]), mpModeler.prod(2.0, var[1])), 20.0, "c1");
  cons[1] = mpModeler.addLe(mpModeler.sum(mpModeler.prod(2.0, var[0]), mpModeler.prod(3.0, var[1])), 200.0, "c2");
}
```

## Summary
Put it together, row-wise model building in CPLEX can be achieved utilizing *IloModeler*, *IloMPModeler* and *IloCplex*.
The process starts with creating decision variables, followed by defining objective function(s) and creating constraints individually.
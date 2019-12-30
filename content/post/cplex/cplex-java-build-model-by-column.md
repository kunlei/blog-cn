---
title: Build Optimization Models by Column using CPLEX Java API
draft: false
date: 2019-10-20
tags: ["cplex", "Java"]
---

In a previous [post]({{< ref "cplex-java-build-model-by-row.md">}}), we looked at how to build models by adding rows sequentially.
It starts with creating decision variables, with which the objective function is defined.
Constraints are then created sequentially and incorporated into the model.
In optimization textbooks, mathematical models are usually represented in matrix format, and in CPLEX, it is indeed possible to build models column-wise.
This post aims to demystify the process of building models by adding columns sequentially.

## An illustrative example

The same illustrative example is repeated here for clarity purpose.

$$
\begin{align}
\text{max.} \quad & 20 x_1 + 15 x_2 \newline
\text{s.t.} \quad & x_1 + 2x_2 \geq 20 \newline
\quad & 2x_1 + 3x_2 \leq 200 \newline
\quad & 0 \leq x_1 \leq 50, x_2 \geq 10
\end{align}
$$


## Column-wise model building

The code below shows an implementation class with two static methods: *main()* and *buildModelByColumn()*.
The former contains a typical model building and solving workflow in CPLEX:

* A CPLEX environment is created by instantiating an object of class IloCplex
* A model is built using functionalities provided by IloCplex
* The model is solved and optimal value is displayed

The latter method details the steps of creating new columns and adding them to a model.
In the case of row-wise modeling building, constraint creation assumes the availability of decision variables.
Likewise, in order to create a new variable and subsequently add it to the model in column-wise modeling, basic model structure must exist.
In other words, we must have an objective function and constraints already defined.
To this purpose, the method starts with creating an empty objective function by employing the method *addMaximize()*.
Two IloRange constraints are also defined using *addRange()* by merely providing their corresponding lower and upper bounds.

With an empty model ready at our disposal, we create new variables in two steps:

1. Create all necessary IloColumn objects and bind them.
2. Create new variables from the IloColumn objects.

*IloColumn* is a abstract class in CPLEX Concert technology that cannot be directly instantiated.
Instead, we must use facility functions defined in IloCplex to obtain objects of IloColumn.
The code below shows two, of possible three, of these functions:

1. column(IloObjective obj)
2. column(IloRange range)
3. column(IloLPMatrix matrix) - we will look at this in future post

You may also notice the function *and()* in the code and it is used to connect multiple IloColumn objects into a composite object.
It is necessary since it is not uncommon for a decision variable to show up in both the objective function and one or more constraints.
With a new column ready, the next step is to create a  new decision variable using *numVar(IloColumn col, double lb, double ub, String name)*
After all the variables are created, the model is ready to be solved.

```java
import ilog.concert.IloColumn;
import ilog.concert.IloException;
import ilog.concert.IloMPModeler;
import ilog.concert.IloNumVar;
import ilog.concert.IloObjective;
import ilog.concert.IloRange;
import ilog.cplex.IloCplex;

public class BuildModelByColumn {

  public static void main(String[] args) {
    try {
      // instantiate a IloCplex object
      IloCplex cplex = new IloCplex();

      // build model by column
      buildModelByColumn(cplex);

      // solve model and display objective value
      if (cplex.solve()) {
        System.out.println("optimal value = " + cplex.getObjValue());
      }
    } catch (IloException e) {
      e.printStackTrace();
    }
  }

  private static void buildModelByColumn(IloMPModeler modeler) throws IloException {
    // create an objective
    IloObjective obj = modeler.addMaximize();

    // create empty constraints
    IloRange[] cons = new IloRange[2];
    cons[0] = modeler.addRange(20.0, Double.MAX_VALUE, "c1");
    cons[1] = modeler.addRange(-Double.MAX_VALUE, 200.0, "c2");

    // create columns and new variables
    IloColumn col0 = modeler.column(obj, 20.0).and(modeler.column(cons[0], 1.0)).and(modeler.column(cons[1], 2.0));
    IloColumn col1 = modeler.column(obj, 15.0).and(modeler.column(cons[0], 2.0)).and(modeler.column(cons[1], 3.0));
    IloNumVar var0 = modeler.numVar(col0, 0, 50.0, "x1");
    IloNumVar var1 = modeler.numVar(col1, 10.0, Double.MAX_VALUE, "x2");
  }
}
```

## Why column-wise modeling is needed

For the small example in this post, column-wise modeling provides no real benefits over row-wise modeling.
It is really personal preference to choose either row-wise or column-wise way of building simple models in CPLEX.
One caveat is that *IloMPModeler* must be used if one wants to build models by column.
*IloModeler* only defines functions for row-wise modeling and lacks the capability to build models by column.

On the other hand, column-wise modeling is necessary in cases like solving problems using column generation, a widely used technique for large scale optimization problems.
Imagine that we have a surfeit of decision variables that it is impossible to include all of them in the model at once, a natural way to overcome this is to only include a portion of variables and gradually identify and add new variables that could improve the objective function.
The fashion of adding variables gradually necessitates the way of column-wise modeling.

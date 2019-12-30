---
title: Build Optimization Models by Row with IloLPMatrix using CPLEX Java API
draft: false
date: 2019-10-19
tags: ["cplex", "Java"]
---

In a previous [post]({{< ref "./cplex-java-build-model-by-row.md" >}}), we examined how to build optimization models row-wise, in which decision variables and constraints are directly created and managed by an IloCplex object.
It turns out that there is another, more powerful in certain cases, way to build and manage constraints in CPLEX and, in this post, we'll look at how to utilize *IloLPMatrix* to build models by row.

## An illustrative example

As usual, let's use a small example to show the case.
It's a contrived model with no practical meanings, and it may not even be feasible.
Five variables are defined in the model, along with three simple constraints and one minimization objective function.

$$
\begin{align}
\text{min.} \quad & 2 x_1 + x_2 + 3x_3 + 2x_4 + 5x_5 \newline
\text{s.t.} \quad & x_1 + x_3 \geq 2 \newline
\quad & x_2 + x_4 + x_5 \geq 3 \newline
\quad & x_1 + x_2 + x_4 \geq 1 \newline
\quad & 0 \leq x_i \leq 100,\ \forall i \in \{ 1, \cdots, 5 \}
\end{align}
$$

## Key steps in using IloLPMatrix

Before getting into the code details, it's worth mentioning that *IloLPMatrix* is a special interface defined in the Concert package to provide a holistic matrix-like representation of constraints and variables.
Note that in our aforementioned post of row-wise modeling, individual constraint is created and subsequently added to IloCplex directly.
IloCplex has all accesses to all modeling components within its space.
Whereas in the case of ILoLPMatrix, they are added to an object of class implementing the IloLPMatrix interface, which acts like a composite object taking over some of the management responsibilities from IloCplex.
In other words, management of constraints is delegated to IloLPMatrix, and this devolvement prevents the constraints and variables from being accessed directly by IloCplex.

As shown in the code below, using IloLPMatrix requires creation of an IloLPMatrix object using either the *LPMatrix()* or the *addLPMatrix()* method, from the IloCplex object. 
The IloLPMatrix object thereafter exists in the space of the IloCplex object and can be used to contain variables and constraints. 
The IloCplex object interacts with all the variables/constraints through the IloLPMatrix object, which provides various functionalities to query and manipulate objects within it.


At this point, the model has an empty matrix with zero row (constraint) and zero column (variable).
The row-wise model building starts with creating decision variables, which we can use *numVarArray()* or *numVar()*.
The caveat here is that the variables must be associated with the IloLPMatrix first, which is achieved by invoking *columnArray()*. 
The function call of *columnArray(IloLPMatrix, int)* in the code returns an array of empty columns, which is further used to create an array of variables.

With the decision variables ready at our disposal, we could construct constraints and here, they are added directly into the IloLPMatrix object, instead of the IloCplex object.
The method *addRows(double, double, int`[][]`, double`[][]`)* is one of the most useful functions provided by IloLPMatrix. 
The first two parameters define the left hand side and right hand side for the constraints, respectively.
The last two parameters specify the coefficient of each variable in each constraint. 

Next the objective function is created by IloCplex, instead of IloLPMatrix. 

```java
import ilog.concert.IloColumnArray;
import ilog.concert.IloException;
import ilog.concert.IloLPMatrix;
import ilog.concert.IloNumVar;
import ilog.cplex.IloCplex;
import ilog.cplex.IloCplex.Param.Preprocessing;

public class BuildModelByRow {

  public static void main(String[] args) {
    try {
      // create IloCplex object
      IloCplex cplex = new IloCplex();

      // add a IloLPMatrix object
      IloLPMatrix matrix = cplex.addLPMatrix();

      // create new variables
      int numVars = 5;
      IloColumnArray colArray = cplex.columnArray(matrix, numVars);
      IloNumVar[] vars = cplex.numVarArray(colArray, 0, 100);

      // add constraints
      double[] lb = {2.0, 3.0, 1.0};
      double[] ub = {Double.MAX_VALUE, Double.MAX_VALUE, Double.MAX_VALUE};
      int[][] indices = {{0, 2}, {1, 3, 4}, {0, 1, 3}};
      double[][] values = {{1, 1}, {1, 1, 1}, {1, 1, 1}};
      matrix.addRows(lb, ub, indices, values);

      // define objective function
      double[] objCoefs = {2, 1, 3, 2, 5};
      cplex.addMinimize(cplex.scalProd(objCoefs, vars));

      // set parameter
      cplex.setParam(Preprocessing.Presolve, false);

      // solve
      if (cplex.solve()) {
        System.out.println("Obj = " + cplex.getObjValue());
      }

      cplex.end();
    } catch (IloException e) {
      e.printStackTrace();
    }
  }
}
```

It happens that the artificial model is feasible, with an optimal objective of 7.

```log
CPXPARAM_Preprocessing_Presolve                  0

Iteration log . . .
Iteration:     1   Dual objective     =             3.000000
Obj = 7.0

Process finished with exit code 0
```

## What's the difference?

Why do we take the extra step to construct an IloLPMatrix object first when building models?
The reason is improved model building speed, especially for large scale optimization models.
The benefit is not obvious for trivial models like the one in this post, but for a model involving thousands of decision variables and constraints, the time saving in the modeling construction step is significant. 
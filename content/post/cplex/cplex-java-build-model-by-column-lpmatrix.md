---
title: Build Optimization Models by Column with IloLPMatrix using CPLEX Java API
draft: false
date: 2019-10-20
tags: ["cplex", "Java"]
---

This post explains how to use IloLPMatrix to build models by column.
IloLPMatrix is commonly used in building large-scale optimization models.

## An illustrative example

The same example used in a previous [post]({{< ref "./cplex-java-build-model-by-row-lpmatrix.md" >}}) is repeated here for clarity.

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

Similar to row-wise model building using IloLPMatrix, the column-wise model building process starts with creating an object implementing the IloLPMatrix interface.
An empty objective function is first defined, followed by the creation of empty constraints using the *addRows()* method by providing their corresponding left hand side and right hand side.

Next, two IloColumnArray objects are created by invoking the *columnArray()* method defined in IloCplex for the IloLPMatrix and IloObjective, respectively. 
They are further combined into a single IloColumnArray object using the *and()* method.
Thew new variables are subsequently created based on the resulting IloColumnArray object.

```java
import ilog.concert.IloColumnArray;
import ilog.concert.IloException;
import ilog.concert.IloLPMatrix;
import ilog.concert.IloNumVar;
import ilog.concert.IloObjective;
import ilog.cplex.IloCplex;

public class BuildModelByColumn {

  public static void main(String[] args) {
    try {
      // create a IloCplex object
      IloCplex cplex = new IloCplex();

      // create an IloLPMatrix object
      IloLPMatrix matrix = cplex.addLPMatrix();

      // create an objective function
      IloObjective obj = cplex.addMinimize();
      double[] objCoef = {2, 1, 3, 2, 5};

      // create empty constraints
      double[] lb = {2, 3, 1};
      double[] ub = {Double.MAX_VALUE, Double.MAX_VALUE, Double.MAX_VALUE};
      matrix.addRows(lb, ub, null, null);

      // add new variables
      double vLb = 0;
      double vUb = 100;
      int[][] indices = {{0, 2}, {1}, {0}, {1, 2}, {1}};
      double[][] values = {{1, 1}, {1}, {1}, {1, 1}, {1}};
      IloColumnArray colArray = cplex.columnArray(matrix, 5, indices, values).and(cplex.columnArray(obj, objCoef));
      IloNumVar[] vars = cplex.numVarArray(colArray, vLb, vUb);

      if (cplex.solve()) {
        System.out.println("obj = " + cplex.getObjValue());
      }

      // close
      cplex.end();
    } catch (IloException e) {
      e.printStackTrace();
    }
  }
  ```

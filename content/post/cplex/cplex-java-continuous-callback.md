---
title: Using ContinuousCallback
draft: false
date: 2019-10-26
tags: ["cplex", "Java"]
---

## Introduction

In this post, we will explore how to use callback, specifically continuous callback, to retrieve and output information during the search process of the simplex and barrier optimizer.
The CPLEX optimizer typically outputs a series of log information to the console revealing some of its search process, and many times it is enough.
Occasionally, we want to have more information, or different information, than those default message, and callback is to the rescue.
We'll examine a simple linear programming (LP) example in this post and use callback to output some exciting information.

The callbacks in CPLEX Java API are nothing special but consist of various abstract classes shipped with predefined methods at our disposal.

## Model

The formulation below involves only two variables and four simple constraints.
The purpose is to make it easy to comprehend in a two-dimensional space. 
The feasible space is a nice octagon in this case.

$$
\begin{align}
  \text{max.} \quad & 2 x_1 + 3x_2 \newline
  \text{s.t.} \quad & x_1 + x_2 \geq 1 \newline
  & -x_1 + x_2 \leq 2 \newline
  & -x_1 + x_2 \geq -2 \newline
  & x_1 + x_2 \leq 5 \newline
  & 0 \leq x_1, x_2 \leq 3
\end{align}
$$

## Code

The code below illustrates the process of creating and using callbacks in a typical model building and solving workflow.
First, an IloCplex object is instantiated, followed by the model building by row using IloLPMatrix.
We turn off the pre-solve step in order to allow Cplex to do the heavy lifting through simplex algorithm.
The default logging is also turned off.

Next, a callback object is created and added to the IloCplex object before we invoke the *cplex.solve()* method.
The class *ContinuousCallbackImpl* is a user-defined callback class that contains the procedure in which our exciting information is retrieved and printed to console.
Note that all it needs to do is to extend the abstract class *IloCplex.ContinuousCallback*, which provides a list of predefined functions at our disposal.

In the example, I want to record the trajectory of objective value during the Cplex search process, for which I create a list suitable for saving pairs of iteration number and corresponding objective value.
The objective value is increasing, as expected, in the trajectory, as shown in the output of the code below.

One thing worth mentioning is that, in the method *main()*, we can directly use lots of functions to retrieve information we want.
These functions are defined in the parent classes.

```java
import com.sun.tools.javac.util.Pair;
import ilog.concert.IloColumnArray;
import ilog.concert.IloException;
import ilog.concert.IloLPMatrix;
import ilog.concert.IloMPModeler;
import ilog.concert.IloNumVar;
import ilog.cplex.IloCplex;
import ilog.cplex.IloCplex.Algorithm;
import ilog.cplex.IloCplex.Param;
import ilog.cplex.IloCplex.Param.Preprocessing;
import java.util.ArrayList;
import java.util.List;

public class ContinuousCallback {

  public static void main(String[] args) {
    try {
      // create a IloCplex object
      IloCplex cplex = new IloCplex();
      
      // build the model by row using IloLPMatrix
      IloLPMatrix matrix = buildModelByRow(cplex);

      // set CPLEX parameters
      cplex.setParam(Preprocessing.Presolve, false);
      cplex.setParam(Param.RootAlgorithm, Algorithm.Primal);
      cplex.setOut(null);

      // initialize callback
      ContinuousCallbackImpl callback = new ContinuousCallbackImpl();
      cplex.use(callback);

      // solve
      if (cplex.solve()) {
        System.out.println("Objective value trajectory: ");
        callback.objs.forEach(System.out::println);
      }
    } catch (IloException e) {
      e.printStackTrace();
    }
  }

  private static class ContinuousCallbackImpl extends IloCplex.ContinuousCallback {
    private List<Pair<Integer, Double>> objs;

    public ContinuousCallbackImpl() {
      objs = new ArrayList<>();
    }

    @Override
    protected void main() throws IloException {
      objs.add(new Pair<>(getNiterations(), getObjValue()));

      System.out.println("Iteration: " + getNiterations64());
      System.out.println("\tCplexTime: " + getCplexTime());
      System.out.println("\tDetTime: " + getDetTime());
      System.out.println("\tStartTime: " + getStartTime());
      System.out.println("\tEndTime: " + getEndTime());
      System.out.println("\tStartDetTime: " + getStartDetTime());
      System.out.println("\tEndDetTime: " + getEndDetTime());
      System.out.println("\tNRows: " + getNrows());
      System.out.println("\tNCols: " + getNcols());
      System.out.println("\t\tObj: " + getObjValue());
      System.out.println("\t\tfeasible: " + isFeasible());
      System.out.println("\t\tdualFeasible: " + isDualFeasible());
      System.out.println("\t\tinfeasibility: " + getInfeasibility());
      System.out.println("\t\tdualInfeasibility: " + getDualInfeasibility());
      System.out.println();
    }
  }

  private static IloLPMatrix buildModelByRow(IloMPModeler modeler) throws IloException {
    IloLPMatrix matrix = modeler.addLPMatrix();

    // create decision variables
    int numVars = 2;
    double[] lb = {0, 0};
    double[] ub = {3, 3};
    IloColumnArray colArray = modeler.columnArray(matrix, numVars);
    IloNumVar[] vars = modeler.numVarArray(colArray, lb, ub);

    // add constraints
    double[] lhs = {1, -Double.MAX_VALUE, -2, -Double.MAX_VALUE};
    double[] rhs = {Double.MAX_VALUE, 2, Double.MAX_VALUE, 5};
    int[][] indices = {{0, 1},
      {0, 1},
      {0, 1},
      {0, 1}
    };
    double[][] values = {{1, 1},
        {-1, 1},
        {-1, 1},
        {1, 1}};
    matrix.addRows(lhs, rhs, indices, values);

    // create objective function
    double[] objCoefs = {2, 3};
    modeler.addMaximize(modeler.scalProd(objCoefs, vars));

    return matrix;
  }
}
```

Below shows the output of the example code.

```
Iteration: 1
	CplexTime: 1.571946910691205E9
	DetTime: 0.001346588134765625
	StartTime: 1.571946910689231E9
	EndTime: 1.0E75
	StartDetTime: 2.288818359375E-4
	EndDetTime: 1.0E75
	NRows: 4
	NCols: 2
		Obj: 2.0
		feasible: false
		dualFeasible: true
		infeasibility: 0.0
		dualInfeasibility: 0.0

Iteration: 2
	CplexTime: 1.57194691069484E9
	DetTime: 0.0025644302368164062
	StartTime: 1.571946910689231E9
	EndTime: 1.0E75
	StartDetTime: 2.288818359375E-4
	EndDetTime: 1.0E75
	NRows: 4
	NCols: 2
		Obj: 4.0
		feasible: true
		dualFeasible: false
		infeasibility: 0.0
		dualInfeasibility: 5.0

Iteration: 3
	CplexTime: 1.5719469106953E9
	DetTime: 0.002979278564453125
	StartTime: 1.571946910689231E9
	EndTime: 1.0E75
	StartDetTime: 2.288818359375E-4
	EndDetTime: 1.0E75
	NRows: 4
	NCols: 2
		Obj: 9.0
		feasible: true
		dualFeasible: false
		infeasibility: 0.0
		dualInfeasibility: 3.0

Iteration: 4
	CplexTime: 1.57194691069592E9
	DetTime: 0.0033283233642578125
	StartTime: 1.571946910689231E9
	EndTime: 1.0E75
	StartDetTime: 2.288818359375E-4
	EndDetTime: 1.0E75
	NRows: 4
	NCols: 2
		Obj: 12.0
		feasible: true
		dualFeasible: false
		infeasibility: 0.0
		dualInfeasibility: 1.0

Iteration: 5
	CplexTime: 1.571946910696485E9
	DetTime: 0.003772735595703125
	StartTime: 1.571946910689231E9
	EndTime: 1.0E75
	StartDetTime: 2.288818359375E-4
	EndDetTime: 1.0E75
	NRows: 4
	NCols: 2
		Obj: 13.0
		feasible: true
		dualFeasible: true
		infeasibility: 0.0
		dualInfeasibility: 0.0

Objective value trajectory: 
Pair[1,2.0]
Pair[2,4.0]
Pair[3,9.0]
Pair[4,12.0]
Pair[5,13.0]

Process finished with exit code 0
```
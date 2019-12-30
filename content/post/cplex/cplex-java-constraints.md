---
title: Cplex Java Concert Technology - Constraints (1)
draft: true
date: 2019-03-06
tags: ["cplex", "java"]
---

Constraint is an essential part of an optimization model, and Cplex provides various ways of defining  constraints in a model.
All constraints in Cplex have a base interface *IloConstraint*, from which other constraint types are derived.
Probably the most commonly used constraint types are *IloRange* and *IloLPMatrix*.


### IloConstraint
*IloConstraint* is a marker interface which extends from *IloAddable* and *IloIntExpr*, both are marker interfaces themselves.
The fact that *IloConstraint* being a *IloAddable* indicates that an object of classes implementing this interface can be added to a model.
*IloConstraint* has several deriving interfaces:

+ IloRange
+ IloLPMatrix
+ IloSOS1
+ IloSOS2
+ IloAnd
+ IloOr
+ IloNumVarBound

IloConstraint can be created using functions from IloModeler:

+ IloConstraint addEq(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint addEq(IloNumExpr e1, IloNumExpr e2, String name)
+ IloConstraint addGe(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint addGe(IloNumExpr e1, IloNumExpr e2, String name)
+ IloConstraint addLe(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint addLe(IloNumExpr e1, IloNumExpr e2, String name)

The functions below create IloConstraints without adding them to the invoking model:

+ IloConstraint eq(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint eq(IloNumExpr e1, IloNumExpr e2, String name)
+ IloConstraint ge(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint ge(IloNumExpr e1, IloNumExpr e2, String name)
+ IloConstraint le(IloNumExpr e1, IloNumExpr e2)
+ IloConstraint le(IloNumExpr e1, IloNumExpr e2, String name)



### IloRange
Objects of classes implementing this interface represent ranged constraints:
$$
lb <= expr <= ub
$$

*IloRange* defines functions to set and query the expression and the bound.

+ void setExpr(IloNumExpr expr)
+ IloNumExpr getExpr()
+ void clearExpr()
+ void setLB(double lb)
+ double getLB()
+ void setUB(double ub)
+ double getUB()
+ void setBounds(double lb, double ub)

### How to Create IloRange?

Constraints are created by functions defined in *IloModeler* and *IloMPModeler*.

##### Functions provided by *IloModeler*
There are four types of IloRange:

+ addEq
    + IloRange addEq(double val, IloNumExpr expr)
    + IloRange addEq(double val, IloNumExpr expr, String name)
    + IloRange addEq(IloNumExpr expr, double rhs)
    + IloRange addEq(IloNumExpr expr, double rhs, String name)
+ addGe
    + IloRange addGe(double val, IloNumExpr expr)
    + IloRange addGe(double val, IloNumExpr expr, String name)
    + IloRange addGe(IloNumExpr expr, double rhs)
    + IloRange addGe(IloNumExpr expr, double rhs, String name)
+ addLe
    + IloRange addLe(double val, IloNumExpr expr)
    + IloRange addLe(double val, IloNumExpr expr, String name)
    + IloRange addLe(IloNumExpr expr, double rhs)
    + IloRange addLe(IloNumExpr expr, double rhs, String name)
+ addRange
    + IloRange addRange(double lb, IloNumExpr expr, double ub)
    + IloRange addRange(double lb, IloNumExpr expr, double ub, String name)

Note that these functions create IloRange **and** add them into the invoking model at the same time.
If all you want is to create constraints without adding them to the model, use the functions below:

+ eq
    + IloRange eq(double val, IloNumExpr expr)
    + IloRange eq(double val, IloNumExpr expr, String name)
    + IloRange eq(IloNumExpr expr, double rhs)
    + IloRange eq(IloNumExpr expr, double rhs, String name)
+ ge
    + IloRange ge(double val, IloNumExpr expr)
    + IloRange ge(double val, IloNumExpr expr, String name)
    + IloRange ge(IloNumExpr expr, double rhs)
    + IloRange ge(IloNumExpr expr, double rhs, String name)
+ le
    + IloRange le(double val, IloNumExpr expr)
    + IloRange le(double val, IloNumExpr expr, String name)
    + IloRange le(IloNumExpr expr, double rhs)
    + IloRange le(IloNumExpr expr, double rhs, String name)
+ range
    + IloRange range(double lb, IloNumExpr expr, double ub)
    + IloRange range(double lb, IloNumExpr expr, double ub, String name)


##### Functions provided by *IloMPModeler*
IloMPModeler provides the following functions to create empty constraints:

+ addRange
    + IloRange addRange(double lb, double ub)
    + IloRange addRange(double lb, double ub, String name)
+ range
    + IloRange range(double lb, double ub)
    + IloRange range(double lb, double ub, String name)
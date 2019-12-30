---
title: Cplex Concert Java Modeling Intefaces - IloModeler
draft: true
date: 2019-04-04
tags: ["cplex", "java"]
---

IloModeler is one of the main modeling interface defined in Cplex Concert Technology.
It is used to create the three key components of a mathematical model, that are, 

+ decision variables
+ constraints
+ objective

### Auxiliary Functions

In addition, cplex provides lots of auxiliary functions:

+ constant
    + IloIntExpr	constant(int c)
    + IloNumExpr	constant(double c)
+ diff
    + IloIntExpr	diff(int v, IloIntExpr e1)
    + IloIntExpr	diff(IloIntExpr e, int v)
    + IloIntExpr	diff(IloIntExpr expr1, IloIntExpr expr2)
    + IloNumExpr	diff(double v, IloNumExpr e1)
    + IloNumExpr	diff(IloNumExpr e, double v)
    + IloNumExpr	diff(IloNumExpr e1, IloNumExpr e2)
+ max
    + IloIntExpr	max(IloIntExpr[] e)
    + IloIntExpr	max(IloIntExpr e1, IloIntExpr e2)
    + IloIntExpr	max(IloIntExpr e1, int val)
    + IloIntExpr	max(int val, IloIntExpr e2)
    + IloNumExpr	max(IloNumExpr[] e)
    + IloNumExpr	max(double val, IloNumExpr e2)
    + IloNumExpr	max(IloNumExpr e1, double val)
    + IloNumExpr	max(IloNumExpr e1, IloNumExpr e2)
+ min
    + IloIntExpr	min(IloIntExpr[] e)
    + IloIntExpr	min(IloIntExpr e1, IloIntExpr e2)
    + IloIntExpr	min(IloIntExpr e1, int val)
    + IloIntExpr	min(int val, IloIntExpr e2)
    + IloNumExpr	min(IloNumExpr[] e)
    + IloNumExpr	min(IloNumExpr e1, double val)
    + IloNumExpr	min(double val, IloNumExpr e2)
    + IloNumExpr	min(IloNumExpr e1, IloNumExpr e2)
+ negative
    + IloIntExpr	negative(IloIntExpr e)
    + IloNumExpr	negative(IloNumExpr e)
+ prod
    + IloIntExpr	prod(IloIntExpr e1, IloIntExpr e2)
    + IloIntExpr	prod(IloIntExpr e, int v)
    + IloIntExpr	prod(int v, IloIntExpr e)
    + IloNumExpr	prod(double v, IloNumExpr e1)
    + IloNumExpr	prod(IloNumExpr e, double v)
    + IloNumExpr	prod(IloNumExpr expr1, IloNumExpr expr2)
    + IloNumExpr	prod(double val, IloNumVar var1, IloNumVar var2)
    + IloNumExpr	prod(IloNumVar var1, double val, IloNumVar var2)
    + IloNumExpr	prod(IloNumVar var1, IloNumVar var2, double val)
+ scalProd
    + IloIntExpr	scalProd(IloIntVar[] vars1, IloIntVar[] vars2)
    + IloIntExpr	scalProd(IloIntVar[] vars1, IloIntVar[] vars2, int start, int num)
    + IloNumExpr	scalProd(IloNumVar[] vars1, IloNumVar[] vars2)
    + IloNumExpr	scalProd(IloNumVar[] vars1, IloNumVar[] vars2, int start, int num)
    + IloLinearIntExpr	scalProd(IloIntVar[] vars, int[] vals)
    + IloLinearIntExpr	scalProd(IloIntVar[] vars, int[] vals, int start, int num)
    + IloLinearIntExpr	scalProd(int[] vals, IloIntVar[] vars)
    + IloLinearIntExpr	scalProd(int[] vals, IloIntVar[] vars, int start, int num)
    + IloLinearNumExpr	scalProd(double[] coefs, IloNumVar[] vars)
    + IloLinearNumExpr	scalProd(double[] coefs, IloNumVar[] vars, int start, int num)
    + IloLinearNumExpr	scalProd(IloNumVar[] vars, double[] coefs)
    + IloLinearNumExpr	scalProd(IloNumVar[] vars, double[] coefs, int start, int num)
    + IloLinearNumExpr	scalProd(IloNumVar[] vars, int[] coefs)
    + IloLinearNumExpr	scalProd(int[] coefs, IloNumVar[] vars)
+ square
    + IloIntExpr	square(IloIntExpr e)
    + IloNumExpr	square(IloNumExpr e)
+ sum
    + IloIntExpr	sum(IloIntExpr[] expr)
    + IloIntExpr	sum(IloIntExpr[] expr, int start, int num)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2)
    + IloIntExpr	sum(IloIntExpr e, int v)
    + IloIntExpr	sum(int v, IloIntExpr e)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3, IloIntExpr e4)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3, IloIntExpr e4, IloIntExpr e5)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3, IloIntExpr e4, IloIntExpr e5, IloIntExpr e6)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3, IloIntExpr e4, IloIntExpr e5, IloIntExpr e6, IloIntExpr e7)
    + IloIntExpr	sum(IloIntExpr e1, IloIntExpr e2, IloIntExpr e3, IloIntExpr e4, IloIntExpr e5, IloIntExpr e6, IloIntExpr e7, IloIntExpr e8)
    + IloNumExpr	sum(IloNumExpr[] expr)
    + IloNumExpr	sum(IloNumExpr[] expr, int start, int num)
    + IloNumExpr	sum(IloNumExpr e, double v)
    + IloNumExpr	sum(double v, IloNumExpr e)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3, IloNumExpr e4)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3, IloNumExpr e4, IloNumExpr e5)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3, IloNumExpr e4, IloNumExpr e5, IloNumExpr e6)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3, IloNumExpr e4, IloNumExpr e5, IloNumExpr e6, IloNumExpr e7)
    + IloNumExpr	sum(IloNumExpr e1, IloNumExpr e2, IloNumExpr e3, IloNumExpr e4, IloNumExpr e5, IloNumExpr e6, IloNumExpr e7, IloNumExpr e8)
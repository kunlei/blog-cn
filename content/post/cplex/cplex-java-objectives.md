---
title: Cplex Java Concert Technology - Objectives
draft: true
date: 2019-03-04
tags: ["cplex", "java"]
---

### IloModeler
Objectives can be added to the model in multiple ways:

+ IloObjective	addMinimize(IloNumExpr expr)
+ IloObjective	addMinimize(IloNumExpr expr, java.lang.String name)
+ IloObjective	addMaximize(IloNumExpr expr)
+ IloObjective	addMaximize(IloNumExpr expr, java.lang.String name)
+ IloObjective	addObjective(IloObjectiveSense sense, IloNumExpr expr)
+ IloObjective	addObjective(IloObjectiveSense sense, IloNumExpr expr, java.lang.String name)

or they can be defined without being added to the model:

+ IloObjective	minimize(IloNumExpr expr)
+ IloObjective	minimize(IloNumExpr expr, java.lang.String name)
+ IloObjective	maximize(IloNumExpr expr)
+ IloObjective	maximize(IloNumExpr expr, java.lang.String name)
+ IloObjective	objective(IloObjectiveSense sense, IloNumExpr expr)
+ IloObjective	objective(IloObjectiveSense sense, IloNumExpr expr, java.lang.String name)


### IloMPModeler
IloMPModeler provides extra ways of defining empty objectives:

+ IloObjective	addMaximize()
+ IloObjective	addMaximize(java.lang.String name)
+ IloObjective	addMinimize()
+ IloObjective	addMinimize(java.lang.String name)
+ IloObjective	addObjective(IloObjectiveSense sense)
+ IloObjective	addObjective(IloObjectiveSense sense, java.lang.String name)

and defining emptying objectives without adding to the model

+ IloObjective	maximize()
+ IloObjective	maximize(java.lang.String name)
+ IloObjective	minimize()
+ IloObjective	minimize(java.lang.String name)
+ IloObjective	objective(IloObjectiveSense sense)
+ IloObjective	objective(IloObjectiveSense sense, java.lang.String name)
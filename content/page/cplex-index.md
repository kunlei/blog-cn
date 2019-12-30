---
title: CPLEX
subtitle: 
date: 2019-08-17
draft: false
comments: false
tags: ["cplex", "java"]
---

This is an index page for all CPLEX-related posts.
<!-- [By column]({{< ref "">}}) -->

## CPLEX Java API

1. [Setup]({{< ref "../post/cplex/cplex-java-setup.md" >}})
2. API deep dive
   1. [IloAddable, IloModel and IloModeler]({{< ref "../post/cplex/cplex-java-api-deep-dive-1.md">}})
   2. IloLPMatrix
3. Model building
   1. By row
      1. [without IloLPMatrix]({{< ref "../post/cplex/cplex-java-build-model-by-row.md">}})
      2. [with IloLPMatirx]({{< ref "../post/cplex/cplex-java-build-model-by-row-lpmatrix.md" >}})
   2. By column
      1. [without IloLPMatrix]({{< ref "../post/cplex/cplex-java-build-model-by-column.md">}})
      2. [with IloLPMatrix]({{< ref "../post/cplex/cplex-java-build-model-by-column-lpmatrix.md" >}})
4. Callback
   1. Optimization callback
      1. [Continuous callback]({{< ref "../post/cplex/cplex-java-continuous-callback.md" >}})
   2. Tuning callback
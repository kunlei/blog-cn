---
title: Dissertation - Service Consistency in Vehicle Routing
date: 2019-04-03
sidebar: false
draft: false
categories:
    - "optimization"
tag: ["research"]
---


This thesis studies service consistency in the context of multi-period vehicle routing problems (VRP) in which customers require repeatable services over a planning horizon of multiple days. 
Two types of service consistency are considered, namely, driver consistency and time consistency. 
*Driver consistency* refers to using the fewest number of different drivers to perform all of the visits required by a customer over a planning horizon and *time consistency* refers to visiting a customer at roughly the same time on each day he/she needs service. 

+ First, the multi-objective consistent VRP is defined to explore the trade-offs between the objectives of travel cost minimization and service consistency maximization. 
An improved multi-objective optimization algorithm is proposed and the impact of improving service consistency on travel cost is evaluated on various benchmark instances taken from the literature to facilitate managerial decision making. 
+ Second, service consistency is introduced for the first time in the literature to the periodic vehicle routing problem (PVRP). 
In the PVRP, customers may require multiple visits over a planning horizon, and these visits must occur according to an allowable service pattern. A service pattern specifies the days on which the visits required by a customer are allowed to occur. 
A feasible service pattern must be determined for each customer before vehicle routes can be optimized on each day. Various multi-objective optimization approaches are implemented to evaluate their comparative competitiveness in solving this problem and to evaluate the impact of improving service consistency on the total travel cost. 
+ Third, a branch-and-price algorithm is developed to solve the consistent vehicle routing problem in which service consistency is enforced as a hard constraint. In this problem, the objective is to minimize the total travel cost. 
New constraints are devised to enhance the original mixed integer formulation of the problem. 
The improved formulation outperforms the original formulation regarding CPLEX solution times on all benchmark instances taken from the literature. 
The proposed branch-and-price algorithm is shown to be able to solve instances with more than fourteen customers more efficiently than either the existing mixed integer formulation or the one we propose in this paper.
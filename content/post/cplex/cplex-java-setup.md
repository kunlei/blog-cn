---
title: Setup and Run CPLEX using Java
date: 2019-10-11
draft: false
mathjax: true
tags: ["cplex", "java"]
---

I have been using IBM ILOG CPLEX for quite some years, both in my research back in graduate study towards my M.S. and Ph.D. degrees and in my industrial projects in the past some years.
Although I have grown more and more familiar with various Cplex functionalities, I still remember the struggles I went through to get it work to solve a simple optimization problem in my first encounter with Cplex.
This post is intended to provide a simple example of connecting Cplex within a small Java program.
Before we get into the details, let me clarify what Cplex is and how it is commonly used.

### Optimization Workflow and Where Cplex Comes into Play
When we talk about Cplex, people may refer to different things. 
Cplex at its core consists of a library of APIs written for programming languages including C, C++, Java, C# and Python.
On top of that, Cplex provides its own *Integrated Development Environment(IDE)* and language named *Optimization Programming Language(OPL)*.
OPL is similar to other mathematical modeling langes like AMPL and is not the solver itself.
A typical workflow of an optimization project may look like this:

1. understand the problem we are trying to solve
2. build mathematical model to optimize certain objectives
3. translate the model into an implementation using OPL, AMPL, C/C++, Java or Python
4. prepare, clean and manipulate data, and feed data into the model implementation
5. revoke optimization solver and show the results

In most, if not all, cases, this is an iterative process in which we may find that some key parts of the problem are misunderstood after reviewing the results, and one has to work through the whole workflow again.
As in other data science fields, the most time-consuming part might not be the model building and implementation steps, but in the data preparation and cleaning phase.
The golden rule 'garbage in, garbage out' always applies, and I learned this in a hard way.
I once spent lots of time debugging through my optimization model trying to figure out why the model produced erroneous results, only to find in the end that the input data was wrong.

It is clear that a modeling language is relevant only in steps 3 and 5, and it's important to keep in mind that Cplex is not the only option here.
There are some other commercial optimization packages, like Gurobi and Fico Express, as well as many other open-source packages, that serve the same purpose.
In my experience, I mainly use Java and C++ to implement optimization models, and in this post, I will use Java to illustrate its interaction with Cplex.


### The Little Optimization Problem that could
I'll use the following artificial optimization problem to show the Cplex implementation in Java.
Let's say we have two decision variables, $x$ and $y$, both within range of $[0, \infty]$, and there is only one constraint: $x + y <= 1$.
The objective is to maximize a function of the form $2x + 3y$.
Put it together, we have
$$
\begin{align}
\text{max.} \quad& 2x + 3y \newline
\text{s.t.} \quad& x + y <= 1 \newline
\quad& x >= 0, y >= 0
\end{align}
$$

### A simple Java example 
To implement the model, I created a java program namaed *TestSetup.java* and put it into directory */Users/klian/dev/cplex-java/src/main/java/com/learn/cplex/setup/*.
Note that I used IntelliJ as my IDE and it automatically created a project directory that follows that Maven file structory.
As a starting point, you can put the source file wherever you want if you don't want to use Maven yet.
The block below shows the source code in *TestSetup.java*.

```java
package com.learn.cplex.setup;

import ilog.concert.IloException;
import ilog.concert.IloLinearNumExpr;
import ilog.concert.IloNumVar;
import ilog.cplex.IloCplex;

public class TestSetup {
    public static void main(String[] args) {
        try {
            // create cplex environment
            IloCplex cplex = new IloCplex();

            // create decision variables
            IloNumVar x = cplex.numVar(0, Double.MAX_VALUE, "x");
            IloNumVar y = cplex.numVar(0, Double.MAX_VALUE, "y");

            // create constraint
            IloLinearNumExpr expr = cplex.linearNumExpr();
            expr.addTerm(1, x);
            expr.addTerm(1, y);
            cplex.addLe(expr, 1);

            // add objective
            IloLinearNumExpr objExpr = cplex.linearNumExpr();
            objExpr.addTerm(2, x);
            objExpr.addTerm(3, y);
            cplex.addMaximize(objExpr);

            // solve and retrieve optimal solution
            if (cplex.solve()) {
                System.out.println("Optimal value = " + cplex.getObjValue());
                System.out.println("x = " + cplex.getValue(x));
                System.out.println("y = " + cplex.getValue(y));
            }
        } catch (IloException e) {
            e.printStackTrace();
        }
    }
}
```

---

### Mac
The Cplex root installation directory on my Mac is */Applications/CPLEX_Studio128/*.
There are two ways to run this example code:

#### Using command line

We have to understand the Java compile and run process if we want to call an external library within our Java program.
The compiling process creates .class files from our .java files, and we run the whole program using the newly created .class files.
Follow the steps below to compile the small Java program:

1. open a terminal and navigate into the directory where the code resides: 

    *cd ~/dev/cplex-java/src/main/java/com/learn/cplex/setup*

2. use command *javac* to compile the *TestSetup.java* program: 

    *javac -classpath /Applications/CPLEX_Studio128/cplex/lib/cplex.jar /Users/klian/dev/cplex-java/src/main/java/com/learn/cplex/setup/TestSetup.java*.

    This command actually works in any directory since we specifies the absolute path to the java source code.

    + It is required that we specify the class path to the Cplex library, otherwise, the compiler cannot recognize class names like *IloCplex* which is not part of Java's builtin package.
    + The compiler will put the generated class files into the same directory of the java files, since we didn't specify the class directory as part of the compiler option.

3. use command *java* to run the program: 

    *java -Djava.library.path=/Applications/CPLEX_Studio128/cplex/bin/x86-64_osx -classpath /Applications/CPLEX_Studio128/cplex/lib/cplex.jar:/Users/klian/dev/cplex-java/src/main/java/ com/learn/cplex/setup/TestSetup*

    We have to provide a few key information to command *java* in order to run our code:

    + the *-Djava.library.path* option specifies the location of the Cplex native library, which is platform-dependent. 
    + the *-classpath* option now includes two pieces of information:
        - the cplex library: */Applications/CPLEX_Studio128/cplex/lib/cplex.jar*
        - the path to our generated class files: */Users/klian/dev/cplex-java/src/main/java/*
        - semicolon **:** is required here
    + the last part of the command gives the package as well as the class name, this is necessary since our class is defined within a package.

    Now we can see the optimization result:
    ```
    Tried aggregator 1 time.
    LP Presolve eliminated 1 rows and 2 columns.
    All rows and columns eliminated.
    Presolve time = 0.00 sec. (0.00 ticks)
    Optimal value = 3.0
    x = 0.0
    y = 1.0
    ```

#### Using IDE
We could save all the hassles of manually compiling Java codes by using an IDE like IntelliJ IDEA (I am using 2019.1 build as of this writing).
All we need to do is 

+ go to *File - Project Structure*, click on *Libraries* under *Project Settings*
+ add Cplex library using the *+* sign
+ add Cplex native library path using the *+* sign

If you are fimiliar with Maven, you just need to add the Cplex dependency in the *pom.xml* file
```java
<dependency>
    <groupId>cplex</groupId>
    <artifactId>cplex</artifactId>
    <version>12.8</version>
</dependency>
```
However, you still need to manually add the native library location even if you use Maven.

---

### Linux
The Cplex root installation directory on my linux machine is */opt/ibm/ILOG/CPLEX_Studio128*.
On a Linux machine, most likely we have to run it via terminal:

1. open a terminal and navigate into the directory where the code resides: 

    *cd ~/dev/cplex/*

2. use command *javac* to compile the *TestSetup.java* program: 

    *javac -classpath /opt/ibm/ILOG/CPLEX_Studio128/cplex/lib/cplex.jar -d /u/users/klian/dev/cplex /u/users/klian/dev/cplex/TestSetup.java*.

    + In my test, I have to specify the *-d* option for the class files, otherwise, the compiler will put the class files in the same directory with the java file, which creates trouble for the step below.

3. use command *java* to run the program: 

    *java -Djava.library.path=/opt/ibm/ILOG/CPLEX_Studio128/cplex/bin/x86-64_linux/ -classpath /opt/ibm/ILOG/CPLEX_Studio128/cplex/lib/cplex.jar:/u/users/klian/dev/cplex/ com/learn/cplex/setup/TestSetup*

    We have to provide a few key information to command *java* in order to run our code:

    + the *-Djava.library.path* option specifies the location of the Cplex native library, which is platform-dependent. 
    + the *-classpath* option now includes two pieces of information:
        - the cplex library: */opt/ibm/ILOG/CPLEX_Studio128/cplex/lib/cplex.jar*
        - the path to our generated class files: */u/users/klian/dev/cplex/*
        - semicolon **:** is required here
    + the last part of the command gives the package as well as the class name, this is necessary since our class is defined within a package.


---


### Windows
The Cplex root installation directory on my windows desktop is */Applications/CPLEX_Studio128/*.





Hope this tutorial helps!

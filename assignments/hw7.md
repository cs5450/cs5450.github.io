---
layout: default
title: Homework 7
nav: assignments
---

##Homework 7

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw7`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile and run the code for your tests/programs in problems 4, 5, 6.
	

###Problem 1 (Red-Black Trees, 10%)

Consider the following initial configuration of a Red-Black Tree:

![](http://bits.usc.edu/cs104/wp-content/uploads/sites/12/2015/03/redblacktree.png)

Draw the tree representation of the Red-Black tree after each of the following operations.  Your operations are done in **sequence**, so your tree should have 17 values in it when you're done. Make sure to clearly indicate each of your final answers.

- Insert 1 
- Insert 13.5 
- Insert 14.5 
- Insert 9

We highly recommend you try to solve these by hand before using any tools to verify your answers.

###Problem 2 (Binary Search Tree Iterators, 20%)

We are providing for you a file `bst.h` (in the homework-resources repository) which implements a simple binary search tree.  You will need to implement the iterator, so that it traverses the tree using an in-order traversal.

You may add any helper functions you like.  A successor helper function (which returns the next largest value after the current node) may be particularly useful.

###Problem 3 (Red-Black Tree Insertion, 30%)

We are providing you a half-finished file `rbbst.h` (in the homework-resources repository) for implementing a Red-Black Tree.  It builds on the file you completed for the previous question.

Complete this file by implementing the `add()` function for Red-Black Trees.  You are strongly encouraged to use private/protected helper functions.

###Problem 4 (Recursion and Backtracking and Boolean Satisfiability, 40%)

An important problem in several contexts (computer-aided design tools, theorem provers, etc.) is boolean satisfiability.  It seeks to answer whether there exists an assignment of 0's and 1's to binary variables {x1, x2, ..., xn} that will
cause a Boolean expression (AND, OR, NOT) to evaluate to true.  The Boolean expression format we will use is called conjunctive normal form (CNF) and an example is shown below:

`(x1 || x2 || !x4) &&  (x3) && (!x1 || !x2)`

We call each term in parentheses a **clause**.  Notice all clauses are AND'ed together but within each clause is the OR'ing of variables.  **For the entire expression to be true, ALL clauses must evaluate to true.**

In this example, the following assignment will cause the expression to evaluate to true:

```
x1 = 0
x2 = 0
x3 = 1
x4 = 0
```

This expression has no assignment that causes it to evaluate to true

`(x1 || x2) && (!x2) && (!x1 || x2)`

The expressions will be given as a text file in a format used by many researches to benchmark their algorithms known as the DIMACs format:

```
c comment here
p cnf num_vars num_clauses
clause 1
clause 2
...
```

The first example we showed above would be represented in the DIMACS format as:

```
c this file mimics the first example above
c assume this file's name is test.cnf
p cnf 4 3
1 2 -4 0
3 0
-1 -2 0
```

Here each variable is represented by it corresponding number (starting at 1) in the clause and negative numbers indicates the variable should be 'NOTed'/inverted (i.e. must be False to make the clause True).  Each clasue ends with a 0 just to 
indicate the end of the line (this isn't really necessary but is part of the DIMACS format) and you can just discard it.  Just to reinforce this:  0 is not a variable but an end of clause delimiter.

Your task is to write a program that reads in one of these DIMACS CNF expressions from a file (provided as the 1st argument on the command line), use a recursive backtracking search to attempt to find a satisfying assignment of 0's (False)
and 1's (True) such that the expression evaluates to true.  Your program should output (as a file whose name is specified as the 2nd argument on the command line) an assignment of variable values if a solution is found 
(you need only find the first solution and not ALL solutions) or `No solution` if no solution exists.

So running the program (using the test.cnf file shown above)

`$ ./sat_solver test.cnf test.out`

should cause an output file, `test.out`, to be generated containing:

```
1 = 0
2 = 0
3 = 1
4 = 0
```

An expression w/o a solution should generate a file whose contents are simply:

`No solution`

Remember you must use a recursive backtracking search approach to get any credit for this problem.  To that end, backtracking search in this context should simply try to start assigning one variable at a time and seeing if
a solution is found, still possible, or not possible.  To do this, each variable should likely have 3 states:  {Unassigned, False, True}.  As you make an assignment to a variable you'd want to check its effect on the formula. 
Recall, since all the clauses are AND'ed, EVERY clause must evaluate to true.  When that happens you know you have a solution.  If even one clause evaluates to false, your assignment is not valid and must backtrack.
Some clauses may not have enough information yet to determine true or false and thus means the current assignment is okay thus far but requires us to make another assignment.
Sometime if we find a solution some variables may not even be assigned yet (meaning their value does not matter).  In that case, simply don't output those unassigned variables to the output file.

Finally, to exercise your RB-Tree implementation above, you **MUST** maintain a map (not an array or list) of each variable to its current value (0, 1, or unassigned), and that map must be your RB-Tree implementation.  Failure to use your RB-Tree implementation for this purpose will result in a deduction of 30% of the possible points for this problem.

Feel free to generate your own CNF files and post them on Piazza along with the solutions.

As always, no memory leaks should be present in your program.


### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw7` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw7 folder `$ cd hw_username/hw7`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.



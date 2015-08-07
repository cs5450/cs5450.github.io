---
layout: default
title: Labs
nav: labs
---

###Lab 9 - Recursive Backtracking

###N-Queens

You should have gone over this in class already, but here's a quick review of the N-Queens problems: You want to arrange 8 queens on an 8x8 chessboard such that each queen is unable to capture any other queens. Now, this can be trivial on a regular-sized chessboard, but what if you wanted to place 100 queens on a 100x100 chessboard? It quickly becomes more complicated.

One algorithmthic solution to this problem is to use recursive backtracking.

###Recursive Backtracking

You should be familiar with some of the uses of recursion at this point. We typically use recursion to split a problem into one (or few) simple, repeatable steps. One popular way to implement a recursive search is to search one step at a time until you hit a dead end (or impossible situation) or find a solution. If you've sucessfully found a solution, you're done. If your recursive call returns unsucessful, remove that value from the list of potential solutions and continue to search until you've exhausted all possibilities.

###Example

This is probably best explained through example. Let's use a 4x4 chessboard for simplicity.

```
1 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0 PASS

1 0 0 0
1 0 0 0
0 0 0 0
0 0 0 0 FAIL

1 0 0 0
0 1 0 0
0 0 0 0
0 0 0 0 FAIL

1 0 0 0
0 0 1 0
0 0 0 0
0 0 0 0 PASS

1 0 0 0
0 0 1 0
1 0 0 0
0 0 0 0 FAIL

1 0 0 0
0 0 1 0
1 0 0 0
0 0 0 0 FAIL

1 0 0 0
0 0 1 0
0 1 0 0
0 0 0 0 FAIL

1 0 0 0
0 0 1 0
0 0 1 0
0 0 0 0 FAIL

1 0 0 0
0 0 1 0
0 0 0 1
0 0 0 0 FAIL (entire row fails)

1 0 0 0
0 0 0 1
0 0 0 0
0 0 0 0 PASS

etc.
```

The first correct solution we will hit is 1, 3, 0, 2.

###Sudoku

[Sudoku](http://www.websudoku.com/) is a popular puzzle game involving a 9x9 grid and the numbers 1-9. The goal of the game is to fill board such that each row, column, and 3x3 box have the numbers 1-9 with no repeats. We will be programming a sudoku solver for lab.

In `sudoku.cpp`, you will find some functions to get you started. Your task is to implement `solveHelper()` called by `solve()`. You may change the parameters as you like. We suggest taking in the row and column of the grid space you're trying to solve.

The basic strategy is as follows:

Start in the top left corner (0, 0) and work your way down to the bottom right corner (8, 8). At each point, check if the block needs to be solved. If the block's value is 0, then it needs to be solved. After you have found a valid number to put in the block, try solving the next one in sequence. Continue until you have solved the puzzle or cannot find a number that will fit in the block.

Compile with `make`, run with `./sudoku`. The first two puzzles should yield a valid configuration, and the last one should fail.

---
layout: default
title: Homework 2
nav: assignments
---

##Homework 2

  + Due: June 5, 2015, 11:59pm (PST)  
  + Directory name for this homework (case sensitive): `hw2`
    - This directory needs its own `README.md` file
    - You should either provide a `Makefile` or step-by-step instructions in the `README.md` file on how to compile the code


###Problem 1 (More git questions, 12%)
In this problem, we will be working with a [Sample Repository](https://github.com/usc-csci104-summer2015/SampleRepo) to measure your understanding of the [file status lifecycle](http://git-scm.com/book/en/Git-Basics-Recording-Changes-to-the-Repository) in git. Please frame your answers in the context of the following lifecycle based on your interaction with the repository as specified below:

![Git File Status Lifecycle]({{ site.url}}/assignments/img/git-file-lifecycle.png "Git File Lifecycle")
>figure courtesy of the [Pro Git](http://git-scm.com/book) book by Scott Chacon

Parts (a) through (f) should be done in sequence. In other words, when you get to part (f), you should assume that you already executed the earlier commands (a), (b), ..., (e). You **must** use the terminalogy specified in the lifecycle shown above, for example the output of `git status` is not accepted as a valid answer. For the purposes of this question, you can assume you have full access (i.e. read/write) to the repository.

####Part (a):
What is the status of `README.md` after performing the following operations:

```bash
#Change directory to the home directory
cd
#Clone the SampleRepo repository
git clone git@github.com:usc-csci104-summer2015/SampleRepo.git
#Change directory into the local copy of SampleRepo
cd SampleRepo
```

####Part (b):
What is the status of `README.md` and `fun_problem.txt` after performing the following operations:

```bash
#Create a new empty file named fun_problem.txt
touch fun_problem.txt
#List files
ls
#Append a line to the end of README.md
echo "Markdown is easy" >> README.md
```

####Part (c):
What is the status of `README.md` and `fun_problem.txt` after performing the following operation:

```bash
git add README.md fun_problem.txt
```

####Part (d):
What is the status of `README.md` and `fun_problem.txt` after performing the following operations:

```bash
git commit -m "My opinion on markdown"
echo "Markdown is too easy" >> README.md
echo "So far, so good" >> fun_problem.txt
```

####Part (e):
What is the status of `README.md` and `fun_problem.txt` after performing the following operations:

```bash
git add README.md
git checkout -- fun_problem.txt
```

Also, what are the contents of `fun_problem.txt`? Why?

####Part (f):
What is the status of `README.md` after performing the following operation:

```bash
echo "Fancy git move" >> README.md
```

Explain why this status was reached.


###Problem 2 (Makefiles, 8%)

####Part (a): 
Every *action* line in a makefile must start with a:

1. TAB
1. Newline
1. Capital letter
1. Space
1. It doesn't matter, any character can start an action line

####Part (b):
Look at [this makefile](https://gist.github.com/alghanmi/eb47fc6151bdb17be249) and answer the following question. Assume this makefile is in the current directory, and all required files are available. Now we run the command 

```
make clean
make shape1
```

Which action(s) will get called? What compiler command(s) with what exact parameters will get executed as a result of the action(s)?

####Part (c):
What is the purpose of a .PHONY rule?

####Part (d):
What are acceptable names for a makefile? Select all that applies.

1. Makefile.txt
1. Makefile
1. makefile.sh
1. makefile


###Problem 3 (Review Material)
Carefully review linked lists (Chapters 4, 9.2) and Recursion (Chapters 2, 5).


###Problem 4 (Linked Lists and Recursion, 38%)
Write a program that reads two lists of integers specified in a text file with integers separated by spaces on two lines (and only two lines) of the file into two singly linked lists.  Then use recursion to:

1. Recursively remove consecutive integers from the first list that are equal (only keeping one)
1. Makes a new list (i.e. copy) such that the new list contains the concatenation of the first and second list **using recursion**

Thus if the input file was:

```
1 3 3 3 5 9 7 7
2 4 4 4
```

you would output:

``` 
1 3 5 9 7 2 4 4 4
```

Each of the two operations above should be implemented as separate recursive functions (you can feel free to define helper function if you want a different signature) that will be called in succession as in:

```c++
removeDuplicates(head1);
head3 = concatenateLists(head1, head2);
```

These recursive functions should not contain any loops.

Using this definition of each item in the list, your two function prototypes should be:

```
struct Item {
  int val;
  Item* next;
};
```

```c++
void removeConsecutive(Item* head);
Item* concatenate(Item* head1, Item* head2);  // returns head pointer to new list
```

You'll need to write the code to read in the integers into the two lists as well.  If you do this in a function you'd need to produce 2 outputs...and you can't return 2 values.  Thus you'd need to pass in the pointers by reference as either:

```
void readLists(Item*& head1, Item*& head2);
```

or

```
void readLists(Item** head1, Item** head2);
```

Feel free to add other arguments to these functions if you need; we're just showing how you'd have to pass the head pointer.

We will run your program as:

`$ ./hw2p3 input.txt output.txt`

Your input should be read from the filename given as the 1st command line argument.  Your output show be stored in the filename given as the 2nd command line argument.

Memory should not be leaked when you remove consecutive equal items and all items should be deleted at the end of the program.


















###Problem 4 (Recursion and Backtracking and Boolean Satisfiability, 50%)

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
Some clauses may not have enough information yet to determine true or false and thus means the current assignment is okay but requires us to make another assignment.
Sometime if we find a solution some variables may not even be assigned yet (meaning their value does not matter).  In that case, simply don't output those unassigned variables to the output file.

Feel free to generate your own CNF files and post them on Piazza along with the solutions.

No memory leaks should be present in your program.

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw1` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw4 folder `$ cd hw_username/hw1`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.
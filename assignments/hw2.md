---
layout: default
title: Homework 2
nav: assignments
---
## HW2

+ Due: TBD
+ Directory name in your github repository for this homework (case sensitive): `hw2`

###Problem 4 (Recursion, Dynamic Memory, 25%)

Write a **recursive** function to split the elements of a singly-linked list into two sublists, one containing the elements smaller than a given number, the other containing the elements larger than the number. At the same time, the original list should not be preserved (see below). Your function must be recursive - you will get only very little credit for an iterative solution.

You should use the following `Node` type from class:

```
struct Node {
    int value;
    Node *next;
};
```

Here is the function you should implement:

```
void split (Node *in, Node*& smaller, Node*& larger, int pivot);
 /* When this function terminates, the following holds:
    - smaller is the pointer to the head of a new singly linked list containing
      all elements of "in" that were less than or equal to the pivot.
    - larger is the pointer to the head of a new singly linked list containing
      all elements of "in" that were (strictly) larger than the pivot.
    - the linked list "in" no longer exists.
```

Hint: by far the easiest way to make this work is to not `delete` or `new` nodes, but just to change the pointers.


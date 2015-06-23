---
layout: default
title: Homework 6
nav: assignments
---

##Project, Part 4

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw8`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a Qt `.pro` file.  We will then just run `qmake` and `make` to compile your `twitter` program.

###Problem 2 (Max Heaps, 10%)

You will answer several questions about Heaps in this problem.

For this problem, you can choose to draw this by hand and scan it (and submit as a JPG or PDF), or to use some graphics software (and produce a JPG or PDF), or draw it using ASCII art (this may be a lot of work).

####Part a (3%)

Indicate whether the following arrays are valid Min Heaps.    For each one which is not, indicate which value(s) are out of place.

- [0, 6, 2, 26, 24, 22, 20, 48, 46, 44]
- [0, 2, 16, 4, 10, 18, 5, 28, 26]
- [0, 2, 16, 4, 10, 18, 26, 28, 5]

####Part b (7%)

Draw the tree representation of the following Min Heap in its initial configuration, and after each operation.  Make sure to clearly indicate each of your final answers.

- Initial Configuration: [0, 2, 4, 6, 8, 10, 12, 14]
- Insert 1 
- Remove 4 
- Remove 0 
- Insert 3

###Problem 3 (m-ary Heaps, 30%)
Build your own m-ary `Heap` class whose public definition and skeleton code are provided in the `hw6` folder of the `homework-resources` with the interface given below.  Rather than specifying a specific type of heap (Min- or Max-Heap) we will pass in a Comparator object so that if the comparator object functor implements a **less-than** check then we will have a min-heap.  If the Comparator object functor implements a **greater-than** check we will have a max-heap.

```
template <typename T, typename Comparator >
class Heap
{
 public:
   /// Constructs an m-ary heap for any m >= 2
   Heap(int m, Comparator c = Comparator());

   // other stuff
   
};
```

You may use the STL `vector<T>` container if you wish.  You should of course **NOT** use the STL `priority_queue` class or `make_heap`, `push_heap`, etc. algorithms.

You should test your heap either using gtest or another test driver program.  Be sure it works as a min-heap and as a max-heap using different comparators.  For reference if your type `T` has a `<` operator, then C++ defines a `less` functor which will compare the type `T` items using the `operator<()`. Similar there is a `greater` functor already defined by C++ that will compare using the `operator>()`.  They are defined in the `functional` header (`#include <functional>` and you can look up their documentation online for further information.  This is meant to just save you time writing functors for types that can easily be compared using built in comparison operators.

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw8` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw8 folder `$ cd hw_username/hw8`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.



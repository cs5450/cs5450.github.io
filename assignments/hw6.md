---
layout: default
title: Homework 6
nav: assignments
---

##Project, Part 3

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw6`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a Qt `.pro` file.  We will then just run `qmake` and `make` to compile your `twitter` program.

###Problem 1 (Class Structures and Inheritance, 20%)

Imagine that you are writing a video game. In it, the player can customize her character in a number of ways:

- there are (currently - future versions might add more) three different types of transportation available: a horse, a ship, and an airship.  A player can own more than one type of transportation.
- the player can customize the color of her transportation.
- there are currently 4 different types of terrain: plains, forest, mountains, water.  Different types of transportation are capable of crossing different types of terrain.
- the player can accumulate arbitrarily many "Items". These currently are
  - Healing Potion
  - Magic Potion
  - Tent
  - Phoenix Down
  - The following Armor: Shield, Helmet, Chain Mail, Gauntlets
  - The following Weapons: Sword, Spear, Crossbow, Axe, Mace
- while a player can accumulate arbitrarily many weapons, at most one can be in use at a given time.

The players also have various characteristics, like strength, defense, and experience. The game supports arbitrary numbers of players playing at any given time.

Diagram the classes involved in the game so far (we will ignore other parts of the game, such as monsters.) Indicate which classes are abstract, and which aren't. Show which classes inherit from each other, publicly or privately. Also show which classes have a "has-a" relationship, possibly using sets or maps or lists where appropriate.

For this problem and the next one, you can choose to draw this by hand and scan it (and submit as a JPG or PDF), or to use some graphics software (and produce a JPG or PDF), or to use UML (if you know it), or draw it using ASCII art (this may be a lot of work). Provide an explanation with your choices, i.e., tell us why you chose to have certain classes inherit from each other (or not inherit). (Notice that you don't need to do any actual programming for this problem.)

Of course, there isn't just one solution, but some solutions are better than others.

###Problem 2 (Comparator Functor, 0%)
The following is background info that will help you understand how to do the next step.  

If you saw the following:

```c++
 int x = f();
```

You'd think `f` is a function.  But with the magic of operator overloading, we can make `f` an object and `f()` a member function call
to `operator()` of the instance, `f` as shown in the following code:

```c++
  struct RandObjGen {
    int operator() { return rand(); }
  };
  
  RandObjGen f;
  int r = f(); // translates to f.operator() which returns a random number by calling rand()
```

An object that overloads the `operator()` is called a __functor__ and they are widely used in C++ STL to provide a kind of polymorphism.

We will use functors to make a sort algorithm be able to use different sorting criteria (e.g., if we are sorting strings, we could sort either lexicographically/alphabetically or by length of string). To do so, we supply a functor object that implements the different comparison approach.

```c++
  struct AlphaStrComp {
    bool operator()(const string& lhs, const string& rhs) 
	{ // Uses string's built in operator< 
	  // to do lexicographic (alphabetical) comparison
	  return lhs < rhs; 
	}
  };

  struct LengthStrComp {
    bool operator()(const string& lhs, const string& rhs) 
	{ // Uses string's built in operator< 
	  // to do lexicographic (alphabetical) comparison
	  return lhs.size() < rhs.size(); 
	}
  };
  
  string s1 = "Blue";
  string s2 = "Red";
  AlphaStrComp comp1;
  LengthStrComp comp2;
  
  cout << "Blue compared to Red using AlphaStrComp yields " << comp1(s1, s2) << endl;
  // notice comp1(s1,s2) is calling comp1.operator() (s1, s2);
  cout << "Blue compared to Red using LenStrComp yields " << comp2(s1, s2) << endl;
  // notice comp2(s1,s2) is calling comp2.operator() (s1, s2);
```

This would yield the output

```c++
1  // Because "Blue" is alphabetically less than "Red"
0  // Because the length of "Blue" is 4 which is NOT less than the length of "Red (i.e. 3)
```

We can now make a templated function (not class, just a templated function) that lets the user pass in which kind of comparator object they would like:

```c++
template <class Comparator>
void DoStringCompare(const string& s1, const string& s2, Comparator comp)
{
  cout << comp(s1, s2) << endl;  // calls comp.operator()(s1,s2);
}

  string s1 = "Blue";
  string s2 = "Red";
  AlphaStrComp comp1;
  LengthStrComp comp2;

  // Uses alphabetic comparison
  DoStringCompare(s1,s2,comp1);
  // Use string length comparison
  DoStringCompare(s1,s2,comp2);
```
	   
In this way, you could define a new type of comparison in the future, make a functor struct for it, and pass it in to the `DoStringCompare` function and the `DoStringCompare` function never needs to change.

These comparator objects are use by the C++ STL `map` and `set` class to compare keys to ensure no duplicates are entered.

```c++
template < class T,                        // set::key_type/value_type
           class Compare = less<T>,        // set::key_compare/value_compare
           class Alloc = allocator<T>      // set::allocator_type
           > class set;
```

You could pass your own type of Comparator object to the class, but it defaults to C++'s standard less-than functor `less<T>` which is  simply defined as:

```c++
template <class T> 
struct less 
{
  bool operator() (const T& x, const T& y) const {return x<y;}
};
```

For more reading on functors, search the web or try [this link](http://www.cprogramming.com/tutorial/functors-function-objects-in-c++.html)

###Problem 3 (Implement Merge Sort, 35%)
Write your own template implementation of the Merge Sort algorithm that works with any class `T`.  Put your implementation in a file `msort.h`.
Your mergeSort() function should take some kind of list (choose either `vector<>`, `list<>`, or `deque<>` based on the needs of your  project).  Your Merge Sort algorithm should also take a comparator object (i.e., functor) that has an operator() defined for it. 

```c++
  template <class T, class Comparator>
  void mergeSort (list<T> or vector<T> or deque<T> myArray, Comparator comp);
	/* you may choose the type of list/vector/deque that fits your design best */
```

This allows you to change the sorting criterion by passing in a different Comparator object.

You should test your code by writing a simple program that defines 1 or 2 functors and then initializes a list/vector/deque with data and finally calls your mergeSort() function.  You should be able to produce different orderings based on your functors.

You can feel free to define a recursive helper function so that your main `mergeSort()` just kickstarts things by calling the helper function.

Finally, integrate this sort function into your Twitter GUI to sort your tweets in the main and @mention feeds from most recent to least-recent.  Essentially, you are no longer allowed to use std::sort()  but must use your own mergesort.

### Problem 4  (Strongly Connected Components, 45%)

A strongly connected component of a graph is a (sub)set of vertices where there exists a path from each vertex to every other vertex.

Given the following relationships in your microblog engine, implement Tarjan's algorithm (do some research to find pseudocode and implement it in C++) to find and output all the strongly-connected components (i.e. of users)
to a file.   **You must use the C++ Stack<T> class in your implementation.**

Add support to your Twitter GUI (i.e. a button and a method to specify an output filename) to compute the strongly connected components of Twitter users.  The format of the output file should have each component titled by number and then all the usernames in that component on a separate line.  A blank line should separate the end of one component and the start of the next.


** Since Tarjan's algorithm requires you to check if an item "exists" in the stack (which is difficult to do with a Stack supporting only push / pop / top, just keep a separate `set` alongside your `Stack` that tracks what elements are in the 
`Stack` and everytime you add or remove a value to the `Stack` you also do the same to your `set`.

Sample output file:

```
Component 1
Jill
Mark

Component 2
Tommy
Billy
```

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw6` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw6 folder `$ cd hw_username/hw6`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.



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


###Problem 1 (More git questions, 6%)
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


###Problem 2 (Makefiles, 4%)

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


###Problem 3 (Review Material, 0%)
Carefully review linked lists (Chapters 4, 9.2) and Recursion (Chapters 2, 5).


###Problem 4 (Linked Lists and Recursion, 25%)
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

`$ ./remdup input.txt output.txt`

Your input should be read from the filename given as the 1st command line argument.  Your output show be stored in the filename given as the 2nd command line argument.

Memory should not be leaked when you remove consecutive equal items and all items should be deleted at the end of the program.


###Problem 5 (Linked Lists, 15%)
We have provided you an incomplete implementation of a doubly-linked list in the `homework-resources/hw2` folder.  You can update/pull the `homework-resources` folder to obtain it and then copy it to your own hw2 directory in your own `hw_usc-username` repo.  For visual reference it is linked here: [llistint.h](https://github.com/usc-csci104-summer2015/homework-resources/blob/master/hw2/llistint.h) and [llistint.cpp](https://github.com/usc-csci104-summer2015/homework-resources/blob/master/hw2/llistint.cpp).  

1. You need to examine the code provided and complete the `insert()` and `remove()` member functions in `llistint.cpp`.  Notice we have provided a private helper function `getNodeAt()` which will return a pointer to the i-th node.  Valid locations for insertion are 0 to SIZE (where SIZE is the size of the list and indicates a value should be added to the back of the list).  Valid locations for removes are 0 to SIZE-1.  Any invalid location should cause the function to simply return without modifying the list.

1. After completing the two functions above you should write a separate program to test your implementation.  You should allocate one of your `LListInt` items and make calls to `insert()` and `remove()` that will exercise the various cases you've coded in the functions.  For example, if you have a case in `insert()` to handle when the list is empty and a separate case for when it has one or more items, then you should make a call to `insert()` when the list is empty and when it has one or more items.  It is important that when you write code, you test it thoroughly ensuring each line of code is triggered at some point.  You need to think about how you can test whether it worked or failed as well. In this case, calls to `get()`, `size()`, and others can help give you visibility as to whether your code worked or failed. 

We have provided a sample test program for you here [testAddToEmptyList.cpp](https://github.com/usc-csci104-summer2015/homework-resources/blob/master/hw2/testAddToEmptyList.cpp).  Use it as a template and good example for how to write tests for individual features of your class.

###Problem 6 (Array Lists, 25%)
Write an integer array-based list class, `AListInt` in two files [alistint.h](https://github.com/usc-csci104-summer2015/homework-resources/blob/master/hw2/alistint.h) (provided for you and reprinted below and `alistint.cpp` (which you will create and implement yourself).  Notice it has the same list interface (public member functions) as the linked implementation above. However, it should also contain a private member function `resize()` that will increase the capacity of the list by **doubling** its size and be called if the list becomes full and the user requests another insertion.


```
#ifndef ALISTINT_H
#define ALISTINT_H

class AListInt
{
 public:
  /**
   * Default constructor - default to a list of capacity = 10
   */
  AListInt();

  /**
   * Another constructor - default to a list to the indicated capacity
   */
  AListInt(int cap);
  
  /**
   * Destructor
   */
  ~AListInt();
  
  /**
   * Standard List interface
   */

  /**
   * Returns the current number of items in the list 
   */
  int size() const;
  
  /**
   * Returns true if the list is empty, false otherwise
   */
  bool empty() const;

  /**
   * Inserts val so it appears at index, pos
   */
  void insert (int pos, const int& val);

  /**
   * Removes the value at index, pos
   */
  void remove (int pos);

  /**
   * Overwrites the old value at index, pos, with val
   */
  void set (int position, const int& val);

  /**
   * Returns the value at index, pos
   */
  int& get (int position) ;

  /**
   * Returns the value at index, pos
   */
  int const & get (int position) const;
  
 private:
  /**
   * Should double the size of the list maintaining its contents
   */
  void resize(); 
   
  /* Add necessary data members here */
  
  
  
};

#endif
```

When you have completed `alistint.h` and `alistint.cpp`, write a test program similar to what you did for the previous problem.  Be sure you test the case where the list is resized. 

###Problem 7 (Stacks, 10%)
Use your linked list implementation from Problem 5 to create a Stack data structure for variables of type int.  Download and use the provided [stackint.h](https://github.com/usc-csci104-summer2015/homework-resources/blob/master/hw2/stackint.h) as is (do NOT change it).  Notice the stack has a linked-list as a data member.  This is called **composition**, where we compose/build one class from another, already available class.  Essentially the member functions of the `StackInt` class that you write should really just be wrappers around calls to the underlying linked list.

You should think **carefully** about efficiency.  **All operations (other than possibly the destructor) should run in (amortized) O(1)**  Failure to meet this requirement may result in the loss of half of the available points on this problem.

###Problem 8 (Dirty Laundry, 15%)
Consider a gym with white and black towels which patrons discard into a can and employees come and collect some number of towels to wash from the top of the pile. Given a file whose contents record the sequence in which towels are discarded along with when and how many towels the employee picks up to wash, please output the sequence of towels the employee picks up to wash each time he visits the can (Note: towel discards and employee pick ups can come in any order).

The input file consists of integers separated by spaces.  `0` = black towel, `-1` = white towel, and any positive number bigger than 0 represents the employee collecting that many towels from the top of the pile.  If there are less towels than the employee tries to pick up, he will just get the available towels.

Sample file contents (e.g. `laundry.in`). There will be no format errors in this file so don't worry about that.

```
0  0  0  -1  2  -1  2
```

Output file contents (e.g. `laundry.out`).  There should be one line per employee pickup of towels.

```
white black 
white black
```

We will run your program as:

`./laundry laundry.in laundry.out` 

and examine the contents of `laundry.out` to check your work.

Use your Stack class to help solve this problem.  **Your solution should run in O(n) where n is the number of integers in the input file.  Failure to meet this requirement will result in a loss of half of the available points of this problem**


### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw2` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw2 folder `$ cd hw_username/hw2`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.

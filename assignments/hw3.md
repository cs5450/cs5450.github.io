---
layout: default
title: HW3
nav: assignments
---

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw3`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile and run the code for your tests/programs in problems 2, 3, 4, and 5.  See instructions in each problem for specific rules.
	
###Skeleton Code
Some skeleton code has been provided for you in the `hw3` folder and has been pushed to the Github repository [`homework-resources`](https://github.com/usc-csci104-summer2015/homework-resources/ ). If you already have this repository locally cloned, just perform a `git pull`.  Otherwise you'll need to clone it.

```
$ git clone git@github.com:usc-csci104-spring2015/homework-resources
```

###Problem 1 (Review Material)
Carefully review Operator Overloading, Copy Constructors, and C++ STL (Interludes 5 and 7 and Appendix I)

###Problem 2 (Copy Constructors and Assignment Operators, 20%)
Modify your `LListInt` class from HW2 to a `LListStr` class that stores strings rather than integers.  Copy your `LListInt` class (.h and .cpp files) from your `hw2` folder to your `hw3` folder and rename the files to indicate you are storing strings.  In addition, add the following `public` member functions:

``` c++
  /**
   * Adds an item to the back of the list in O(1) time
   */
  void push_back(const string& val);
  
  /**
   * Copy constructor
   */
  LListStr(const LListStr& other);

  /**
   * Assignment Operator
   */
  LListStr& operator=(const LListStr& other);
```

You should write a Google Test-based unit test program (using your knowledge of unit-testing) that ensures each of these functions work and also use `valgrind` to check that there are no memory leaks.  Add a rule `copytest` to your `Makefile` to compile all the needed code and your test program.  We should be able to compile your program by simply typing `make copytest`.

### Problem 3 (Sets and Operator Overloading, 35%)

#### Overview 
Write a set class that can store integers.  We have provided a `setstr.h` that you should use in the `homework-resources` repository.  If you update (git pull) on that repo which you should already have, you should find a `hw3` folder with the header file in it.  Copy it into your own hw_username/hw3 folder.  

#### Basic Implementation (10%)
To implement the set, make its primary data member your `LListStr` class that you completed in the previous problem (be sure to add necessary #include statements as well). Many `set` functions can be easily implemented by simply utilizing the list functionality you wrote in `LListInt` (i.e. calling member functions of the list data member).  But recall a set cannot contain duplicate data values.

Start by implementing the basic functionality of `empty`, `size`, `insert`, `remove`, and `exists`.  Adhere to a few notes:

1. If a client attempts to insert a value that already exists, the call to `insert` should have no effect
1. If a client attempts to remove a value that does not exist, the call to `remove` should have no effect

Think carefully if you need to add a copy constructor and assignment operator.  Remember the rule of 3 [if you need your destructor to actually deallocate memory, then you likely also need a copy constructor and assignment operator].  If you believe your class should have an explicit copy constructor and assignment operator, add it now.

#### Iteration Ability Implementation (10%)
Add a simple iteration capability to your set class to iterate over the internally stored data.  You should do this by implementing two functions:  `first` and `next`.  `first` should return a pointer to the 1st data item in the set (the pointer should be constant since you shouldn't be able to change the data with that pointer) and do any setup to allow future calls to `next` to return pointers to each subsequent data item in the set.  If the set is empty, `first` should return NULL.  As `next` is called it should return pointers to the 2nd, 3rd, etc. items in the set in sequence.  When `next` is called and no more items are left to iterate over, return NULL.

``` c++
  /**
   * Return a pointer to the first item
   *  and support future calls to next()
   */
  string const* first();

  /**
   * Return a pointer to the next item
   *  after the previous call to next
   *  and NULL if you reach the end
   */
  string const* next();
```

Hint:  Use the functionality of the list and some private data member in your set class to track where you are in the "iteration" process.

**Note**:  As a requirement for the client, if they modify (insert or remove from) the set in between calls to `first` and `next` or successive calls to `next` the iteration process may be corrupted.  Thus any modification to the set should require the client to reinitialize the iteration process by calling `first` again and starting over. In other words, if someone modifies the set during iteration and keeps calling `next`, your codes behavior may be undefined (i.e. it's not your fault if it crashes!!!).  But if they don't modify the set during iteration or call `first` again after modification, your code should work without fault. 

#### Union, Intersection, and Operator Overloading (15%)
Now add member functions to compute the set union and intersection along with appropriate operator overloads ( `|` = union and `&` = intersection ).  These operations should result in a new (though not dynamically allocated) set being returned.  You should be able to invoke these operations either with their function call or the operators as shown below:

``` c++
SetInt s1, s2, s3;
// operations to insert values to s1 and s2

// These two should behave equivalently
s3 = s1.setUnion(s2);
s3 = s1 | s2;  

// As should these
s3 = s1.setIntersection(s2);
s3 = s1 & s2;  
```

###Problem 4 (Unit Testing, 15%)
Use Google Test to write unit tests for your Set member functions: `insert`, `remove`, `exists`, `setIntersection`, and `setUnion`.  Add a rule `settest` to your `Makefile` to compile all the needed code and your test program.  We should be able to compile your program by simply typing `make settest`.  We will then run it with the command `./settest`.  


###Problem 5 (STL Maps, 30%)
Write a program to read in student names and their major(s) and then allows queries as to which students have declared a certain major or double major.  

We will provide you an input file with a student's name followed by a comma (though there may be whitespace like ' ' or '\t') and then the 1 or more majors that they have declared.  Each student record will appear on a single line.  A blank line should just be skipped.

We will then provide a command file where each line contains 1 or more majors and we want to know what students have declared that major, double major, triple major, etc.  

Thus your program should read in the data from the input file and store it in a **map** (you should use the C++ STL map class) to allow for efficient computation of what students have declared a given major to produce the answer to the queries from the command file.  Consider what you would use as the key and what you should use as the value in your map.  Recall, the key represents the information you have and the value represents the information you want to access.  For double or triple majors consider how your set operations (intersection or union) might be useful to you (thus, you should use your `SetStr` class in this problem). 

Below is an example of the input files and output file produced by your program.  **We have added the sample input file and command file to the `homework-resources/hw3` repository.**

Sample input file (majors1.in)
```
Mark W. Redekopp , CECS
Mark W. Redekopp , EE
Connor Gorman, CSCI
Merrick Bautista	, Cecs
Merrick Bautista,BISC
David Kempe, CSCI
Student Athlete, comm
Aaron Cote, CSCI HRTHS
Max Nikias, Buad EE BUAD FNDR
Steven B. Sample, BUAD EE
CS Minorville, BUAD
Mike Zyda, CSGM
Joe Genius, PHCS
David Pritchard, CANST CSci
```

Sample command file (majors1.cmd)
```
CECS 
EE
ISE
Buad
EE BUAD
```

We will run your program as:
 
`./majors majors1.in majors1.cmd majors1.out`

Sample output file produced by your program (majors1.out)

```
CECS
Mark W. Redekopp
Merrick Bautista

EE
Mark W. Redekopp
Max Nikias
Steven B. Sample

ISE

BUAD
CS Minorville
Max Nikias
Steven B. Sample

EE BUAD
Max Nikias
Steven B. Sample
<p>
```

####Additional Notes and Requirements

 - A student might be listed on multiple lines to indicate his/her multiple majors or majors might be all on a single line with the student's name (see the input file for examples).
 - Remember a comma will separate a student name from their majors but there may be whitespace (spaces or tabs) between the student name and comma and the comma and the major(s). For example, in the input file there is a [TAB] ('\t') after the first line with Merrick's name.    
 - The majors should be treated as case-insensitive.  Thus, `Buad` represents the same major `BUAD` or `buad`.  
 - The output file should reprint the majors being queried on one line followed by the students who match the query on the subsequent lines (one per line).  Leave a blank line after the last student's name.
 - The filenames should be taken from the command line in the order shown above (input file, command file, output file).
 - You must use a **map** to store your information and use your `SetStr` class where appropriate
 - **Hint:** To help with the above requirements, the `cctype` library has functions such as `ispunct()`, `isspace()`, `tolower()`, `toupper()`, etc.
 
Add a rule `majors` to your `Makefile` to compile all the needed code.  We should be able to compile your program by simply typing `make majors`.  We will then run your program at the command line.


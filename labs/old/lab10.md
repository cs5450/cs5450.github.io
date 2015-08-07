---
layout: default
title: Labs
nav: labs
---

###Hash Tables & Doxygen

###Doxygen Introduction

Doxygen is the de facto standard tool for generating documentation from annotated C++ sources, but it also supports other popular programming languages such as C, Objective-C, C#, PHP, Java, Python, IDL (Corba, Microsoft, and UNO/OpenOffice flavors), Fortran, VHDL, Tcl, and to some extent D.

###How do we use doxygen?

To generate the documentation:

```
$ doxygen -g lab10.doxy
$ doxygen lab10.doxy
```

To view it:
```
$ firefox html/index.html &
```

Doxygen will look through your code and create documentation out of it. Other people using your code should be able to read the documentation and know how to use the functions you have written without actually reading the code. You should be somewhat familiar with reading documentation for things such as [C++](http://www.cplusplus.com/) or [Qt](http://doc.qt.io/qt-5/reference-overview.html).

Doxygen specifically looks for comments starting in `/**` right before class functions. There are also special keywords you can provide doxygen so it can note attributes of a function such as parameters and the return value. Here's an example for the HashTable constructor:

```
/**
Creates a hash table of the given input size. Creates the array and vectors used for the hash table.

/param size The size of the hash table.
*/
```

We include that there is a parameter called "size" that defines the size of the hash table.

###Hash Tables

Speaking of hash tables! We'll be writing a simple hash table with chaining for this lab. The hash function is already provided. Let's briefly review how a hash table works.

Hash tables take advantange of the O(1) access time of arrays. They require some kind of mapping from the key to the index in the array (whether it's the integer value of the key, its address in memory, or a more complicated calculation). Hash table implementations try to balance fast access times with limited storage space. In an idealistic situation, our hash function would never generate collisions in the hash table, but unfortunately the real world isn't so simple.

###Hash Function

We've provided a hash function for you. It takes in a char * and outputs an unsigned number. The same char * sequence will always output the same unsigned number, so you may use this number (mod the size) as the index of the array in which to insert the value.

###Chaining

We'll be using chaining to account for collisions. The hash table given has an array of vectors of strings. Sounds complicated, but it's not so bad. Looking up an index in the array gives a vector of strings (our hash table stores strings). This way, if we encounter a hash collision, we simply add our value to the vector and we're done! Make sure your find() and remove() functions also realize there could be more than one value in the vector.

###Check Off

Here's what we're looking for when we check you off:

* Doxygen documentation of the 3 functions you need to write (insert, remove, find).
* Everything in the main program runs with GOOD.

Here's some review questions:

* What's the worst-case runtime of our hash table?
* What's one improvement you can make to the hash table?
* Was there a collision in our hash table? How do you know?
* What is the .doxy file used for?

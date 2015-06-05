
---
layout: default
title: Labs
nav: labs
---

What is a Copy Constructor?  A class constructor that is used for creating a new object as a copy of an existing object.  For example, a function declaration of a copy constructor may look like this:

```
Example(const Example& e);

```

What is Operator Overloading?  There are many symbols that denote certain functions in C++, like addition (+) or subtraction (-).  It's easy to see how a programming language knows to output '4' when you pass it '2+2' or even that "cat" + "dog" gives "catdog", but how would the programming language react if you created a class Car, and performed the operations car1 + car2?  Operator overloading allows the programmer to specify what should happen in these situations (if anything should happen).  To overload the operator '+' symbol, one would declare a function like this:

```
Example Example::operator+(const Example& e);

```
 

STL Map?  A map in computer science is generally referred to as a data structure that holds the abstraction of containing pairs of items.  Each pair has a key and a value.  STL simply refers to the C++ standard library.

In this lab, you will learn to use the STL Map class and use it to create your own copy constructor and overload the '+' and '=' operator.

AGENDA:

 - How to use STL Maps
 - Shallow vs. Deep Copy - Copy Constructor
 - Create an Assignment Operator
 - Create a Plus Operator

 
#### Part 0 - Introduction

Part 0 explains how to use an STL Map.  No code is required, but you will likely reference this section during your lab.  You can also use this part of the lab as a reference for later homeworks.

### Iterating Over a Map

This lab will entail a heavy amount of iterators, so let's quickly review what you may have learned in lecture.

Generally speaking, to receive an iterator object pointing to the first element in a data structure, one uses the begin() function.

To get the element after the last element (essentially some NULL value, since it doesn't exist), one uses the end() function.

STL Iterators also overload the operator '++' to represent the next() function.  What does this operator usually do?  It increments by one.  Another way of thinking of the '++' operator is that it goes to the next value.  

So that's great, we know how to iterate over the data structure, but how do we retrieve the value that the iterator is currently pointing at?  There can be multiple ways, but for our STL Map, we'll access the current pair we're pointing to with 'first' and 'second' (since a single element in a map holds two items, a key and value).

Let's see what a simple iteration with an STL Map container might look like:

```
std::map<std::string, std::string>::iterator it;
for(it = myMap.begin(); it != myMap.end(); ++it)	 
{
    std::cout << it->first << std::endl;
    std::cout << it->second << std::endl;
}

``` 

### Inserting into a Map

Two basic methods exist for insertion.  There is  insert() , which will only add the specified element if the key does not already exist.  If the key does exist, it does nothing.

The second is using the operator overloaded  [] to index into the map.  Programmers must specify a key as an "index", and then assign a value.  Unlike insert() , using [] will overwrite the value if the key exists.  Examples are below:

```
myMap.insert(std::make_pair("Key", "Value"));    // Because each element in a map is a pair, one must insert a pair
myMap["Key"] = "Overwrites Previous Value";

``` 

### Finding Elements in a Map

To find an element, one can use the  find()  function.  This returns an iterator pointing to the specified pair object, if found.  If it wasn't found, it returns a pointer to end(). For example:

```
std::map<std::string, std::string>::iterator it = myMap.find("Key");

```
One can also use the operator  [] to retrieve the element if the key exists.  For example:

```
std::string var = myMap["Key"];

 ```

### Removing Elements in a Map

To remove a single element, one will use the  erase() function.  It accepts an iterator or a key as a parameter.  To delete the key  "Key" , which we inserted in the above examples,  we perform one of the following operations:

```
myMap.erase (myMap.find("Key"));   // find() returns an iterator pointing to the object containing "Key"

```
or, simply:

```
myMap.erase ("Key");   // This first searches for an object containing "Key", then removes it

```

To remove all elements, use the  clear() function.

 
#### Part 1 - Walkthrough

Let's first begin by quickly identifying what we're dealing with in this lab.  A modern fast food restaurant wants to up its game and have its ordering system completely automated, becoming the fastest fast food restaurant around.  Despite their future thinking, they're not that great with code and want to have everything be as simple as possible.  Our job is to create an Order class that handles the adding of multiple Order objects and the copying of one Order to another.

Navigating to the lab5 directory should show the following files and folders:

```
lib/  main.cpp  Makefile  README.md  src/

```

The only file you will need to touch for this lab is src/order.cpp.  Every other file is already completed. By opening order.cpp, you will see there are several functions left blank.

Note: Performing make will correctly compile the files. At this point, running with ./bin/lab5 will bring up a menu, but seg-fault if any option is pressed.  Adding a copy constructor will resolve this issue.

 

### Copy Constructors - Shallow vs. Deep Copy

By default, C++ generates a default copy constructor which takes each variable from an object and assigns it to the current object.  This is called member-wise assignment. Let me provide an example:

```
// By default, C++ will do this without you ever specifying a copy constructor
Example::Example(const Example& e)
{
    this->e1 = e.e1;
    this->e2 = e.e2;
    ...
    this->eN = e.eN;
}

```

For many cases, this may be fine.  But when it comes to pointers, one can run into bad issues.  Can you think of why?

The answer is shallow copy.  Usually, when copying, we want to copy contents of one object to another object.  In the case of the default copy constructor, it may end up equating one pointer to another, thereby having two pointers pointing to the same value.  In other words, rather than copying the content over, the default constructor assigned the current object to point to values in the other object!  Think about why this may be bad.  Ask a question if you are unsure.

Actionable task: Let's fill out the copy constructor for Order in  `order.cpp` .  Every Order contains a variable  `map<string, int>* items` , so we'll need to perform a deep copy.  To do so, we'll manually copy each item and add it to our own map.  The code for this is below:

```
    Order::Order(const Order &rhs)
    {
        // items = rhs.items;    // Shallow copy! Not what we want.
        items = new std::map<std::string, int>();    // Allocate new map
        std::map<std::string, int>::iterator it;
        for(it = rhs.items->begin(); it != rhs.items->end(); ++it)  // For each item in RHS
        {
            items->insert(std::make_pair(it->first, it->second));
        }
    }

```

Let's quickly analyze this.   The code dynamically allocates a new map.  It then goes on to iterate over every element in the Order that was passed in (the one we want to copy from).  With each element from  Order rhs , we are provided a  first and  second value.  We use those values to add a new pair element into our current items map using the function `insert()`.

Now running the program with  make and  `./bin/lab5` should allow all menu options to be entered without seg-faulting (though this program is still far from finished).

 
#### Part 2 - Overloading

In this part of the lab, you will be doing the leg-work.  There are three functions to complete:

 - clearOrder() --> removes all items for the map
 - operator=() --> assigns one Order object to be the equivalent of another Order object
 - operator+() --> returns the combination of two Orders
  - Ex: Order 1 has an order for 1 Burger, Order 2 has an order for 1 Burger and 2 Fries. The resulting Order has an order for 2 Burgers and 2 Fries.

It is suggested you complete them in the order declared.  Aim for no memory leaks!

Helpful explanations to help you complete part 2 are provided below.

 

### How to Run the Code

```
$ make clean
$ make
$ ./bin/lab5

```
 

### Assignment Operators

Like copy constructors, assignment operators are created by default following the same principles (member-wise assignment).  But, because it is not a constructor, an object must be returned.  An example, similar to that of the copy constructor, looks like this:

```
// By default, C++ will do this without you ever specifying an assignment operator
Example& Example::operator=(const Example& e)
{
    this->e1 = e.e1;
    this->e2 = e.e2;
    ...
    this->eN = e.eN;
    return *this;
}

```

This function will be triggered whenever the programmer attempts to do something like below:

```
Example e1;
Example e2;
e2 = e1;  // Same as e2.operator=(e1);

```
 

### Binary Arithmetic Operators

Unlike the assignment operator, this should not modify the values at hand.  Instead, it should return the combination of the two elements.  For example, 1+2 evaluates to a value of 3, but 1 still holds its own value of 1 and 2 still holds its own value of 2.  The values of the two elements are unchanged, but what is returned is an entirely separate object with a (potentially) new value.  Here's an example of adding two vectors together:

```
Vector3 Vector3::operator+(const Vector3& rhs)
{
    Vector3 lhs = *this; // uses copy constructor
    lhs.x = rhs.x + lhs.x;
    lhs.y = rhs.y + lhs.y;
    lhs.z = rhs.z + lhs.z;
    return lhs;
}

```

Further reading: Operator Overloading Guidelines

 

### To get checked off

Explain how you implemented the three functions.

Note: If you implemented the operator+() function correctly,   Vegetables will never be added to the Order, even if attempted.  Say thank you to our future-thinking fast food restaurant.

This single run with these menu inputs should have these outputs:

```
6
Output: Empty

1
2
3
4
5
6 
Output:
Burger 1
Fries 1
Drink 1

7
6
Output: Empty

5
6
Output:
Burger 1
Fries 1
Drink 1

1
6
Output:
Burger 2
Fries 1
Drink 1
```

 

 

 
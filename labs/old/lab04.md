---
layout: default
title: Labs
nav: labs
---

### Lab 4 - Unit Testing and GTest

We've already experimented with testing programs in HW2, and in this lab we want to dive into the topic of unit testing a little bit more. The goal of this lab is to give you an overview of how to use Google Test framework to build and run test cases for any C++ project.

### Motivation

Testing is absolutely essential to a typical development process. Having a complete test suite can ensure that the code that you spent hours building works, fits specification, and is bug-free.

Suppose you have just finished homework and you think you have a pretty good implementation of a doubly linked list. How do you know, for sure, that this list behaves like how you intended it to be? You can manually read through the code a million times, but you might still miss something.
 
A good test case program should:

- Use your newly created data structure
- Perform simple operations with your data structure's methods
- Make sure that each method performs the way you intended it to be
 
There are several advantages for having a well-crafted test program: 

- Help you think about edge cases - ones that you might not have thought of when implementing the data structure itself.
-  Help you make sure each subpart of a problem works correctly. For example, when you implement a new data structure with your new LinkedList, and you encounter a problem, it's very hard to know whose fault it is - the new data structure or the underlying LinkedList. By thoroughly testing your LinkedList in its corresponding test, you will be able to nail down where to hunt for problems fast.
- When you make a change to your code, you can make sure that it did not break anything.

The [Google Test documentation](https://code.google.com/p/googletest/wiki/Primer) has further guidelines for what constitute good test suites.


## 1 - Testing a Fibonacci Program

### 1.1 Introduction to Testing

Let's first begin writing tests for a simple function that computes the nth Fibonacci number. But before you open the code for the Fibonacci program, ask yourself - what are some test cases that we'll need to test?

**Nominal / spec path cases** - You can always start with the most basic cases - that 5th fibonacci should be 5, 7th should be 13. It's a good idea to include a few basic cases so to be sure you weren't just lucky, but you shouldn't need anything more than that. 

**Boundry cases** - What are the boundaries for this function? For a Fibonacci calculation, it seems that there is only a "lower-bound", namely when n = 1. For some data structures that are slightly more complex, there might be multiple boundaries, like the resize limit ("`capacity`") for ArrayList.

**Off-nominal cases** - Your program should be completely fool-proof. That means your program should handle even input that does not make sense. For example, asking for the -2th Fibonacci number, or asking to insert at the 5th position for a 2 element array. 

Notice that we came up with these cases **without the need of looking at code** - all we did was think about how a Fibonacci program should behave, and how a user would be using such a function. In fact, it's very common for industry software engineers to write the test cases first _before_ writing a single line of code; they call this [Test-Driven Development (TDD)](http://en.wikipedia.org/wiki/Test-driven_development).

A very simple test program might include a long list of test cases made of if statements that looks like this:

```
Fibonacci fib;
if (fib.get(5) == 5) {
	std::cout << "OK" << std::endl;
} else {
	std::cerr << "FAIL | 5th Fibonacci number should be 5."
}
```

However, a long list of these cases can get out of hand very quickly - it's annoying to read and even more so to write repetitive code. We can use a library called Google Test to help us out. 

#### What is enough, and how much is too much?

Generally speaking, there should be at least one test case for each code execution path in a function. For example, if a function has 3 branch conditions, test at least each path once, to ensure all use cases are covered. There is no need to test a single path more than a few times - it doesn't hurt, but it's generally unnecessary. This is only a rule of thumb, as some edge cases just cannot be predicted by looking at the code. 

When testing a class implementation, each public function should be tested. For example, an `ArrayList` should have test cases for each of `add()`, `set()`, `remove()`, `get()`, and so on. More on class testing in a little. 

### 1.2 Our First Google Test Case

Navigate to the `part1` directory, and open the `test.cpp` file included for you. The meat of the test suite provided to you in this file is in these four lines:

```
TEST(FibTest, Nominal) {
	Fibonacci f;
	EXPECT_EQ(f.get(5), 5);
	EXPECT_EQ(f.get(7), 13);
}
```

You should notice that this is not standard C++ syntax. We're using a **C++ Macro** here that is supplied by the framework. C++, when compiling, will automatically replace this code with some appropriate C++ code defined in the framework. It is likely a much longer chunk of code that Google Test wants to hide and save the user from an unnecessary amount of copy pasting. 

A test case is defined by calling this `TEST` macro. Two more things are important here: `FibTest` and `Nominal`, the two "parameters" that are "passed into" the Macro. The first parameter specifies the name of the test suite. As a rule of thumb, all tests in a file should belong to the same test suite, and in this case, FibTest. The second parameter is the name of this test. We're testing the trivial cases right now, so we're just going to call it the "Nominal" case. 

```
EXPECT_EQ(f.get(5), 5);
```

Another macro here - `EXPECT_EQ`. The name of this macro is not too surprising - test cases are but a long list of expected results under different circumstances. In here, by calling `EXPECT_EQ`, you're saying: "When I call f.get(5), the value it returns should equal to 5."

### 1.3 Adding some more test cases

As we've mentioned, there are more test cases worthy to be tested. Let's add them together to our `test.cpp` file. 

#### Boundary Case

In our Fibonacci calculation, we have a base case of "1" and "2" - they return special values unlike the nominal cases. We should add a test that makes sure this happens.

We first start with a call to the macro, with the test suite name staying the same (`FibTest`), and the test case name something like ("Boundary").

```
TEST(FibTest, Boundary) {
	Fibonacci f;
	...
}
```

What goes inside? Expectation statements. What do you expect `f.get(1)` return? `f.get(2)`?

```
	EXPECT_EQ(??, ??)
	EXPECT_EQ(??, ??)
```

When you're done - _don't run the tests yet_! Finish the entire test suite first, beofre you try to see if anything's wrong. This is to prevent us tailoring our test cases to the code - you would be able to think about edge cases much more easily from a different perspective.

#### Off-Nominal Case

What else can go wrong? A rule of thumb is: never trust the user. If something can go wrong, it will; so it's essential that your program handles correctly. 

For the sake of this testing demonstration, let's assume that any invalid input should return a value of 0. Some invalid inputs in this case could be 0, -1. 

Write a test case for this and include it in `test.cpp`.

**Bonus**: There is one more edge case. Can you think what it is? (Hint: What's the type of the input?)

### 1.4 Run the tests now

Now that we have a complete test suite, we can see if our program works fully to our specs by running `make tests` on the terminal. Because our dependencies are set up properly, this will attempt to compile the `fib` object, and then the test executable. After all the compiling is done, it will then attempt to run the test executable.

You should now see a fancy test runner output, that says the output is unexpected, followed by a segfault. It's okay, we'll fix them one by one.

```
// Line 5: Change from:
return 0;
// to:
return 1;
```

Run the tests again by calling `make tests`. That nominal test case now passes successfull, but the segfaults are still there in the Off-Nominal Case. Well, that's unexpected. As always, try to debug using gdb.

```
gdb bin/fibTest
```

Run until the segfault happens, and then run `bt` to see how the error occurred. It seems that the program crashed becuase we ran out of stack space by calling too many recursions. Fix this by modifying our fib.cpp:

```
// Add after Line 3:
if (n <= 0) { 
	return 0;
}
```

Run the test again, and it should now pass with flying colors - green, that is.

## 2 Testing a Class

Now that we understand how to run tests, we're going to practice testing on a class implementation. In particular, this is an ArrayList class, like the one you've finished in HW2.

### 2.1 Understanding Fictures

When we test classes, we often need some initialization before we can test. For example, initializing classes with the correct constructor parameters, populating an array, etc. Because these setup are usually repeated across multiple test cases, we put them in "Fixtures"

Open the `test.cpp` in the `part2` folder. You'll notice that this file is slightly longer than the one in the first part. The main difference being a definition of a class `ArrayListTest`. There are several characteristics to this class:

- It inherits from a Google Test class `testing::Test`. We have not learned what inheritance works yet, but you will understand how it works in a little bit
- The class is largely empty, but it includes a declaration of a member variable of type `ArrayList`. This object is available for use in any of the test cases
- It has four methods you can insert code in: 
	- the class constructor and `SetUp()` is largely the same:both will be called before each test case
	- similarly, the class destructor and `TearDown()` will be called after each test

In order to have access to the fixture, tests need to be declared using the `TEST_F` fixture instead of `TEST`. The test suite name should also be the same as the class name.

You can see all that comes to play in the sample `get()` test. Notice that we are able to read elements from the list right away without any other code inside the test. In fact, for every test in this test suite will inherit these setup instructions as well. This helps to keep the actual test code simple.

### 2.2 Lab Assignment

**TL;DR: Find 2 out of 3 bugs by writing tests, and ask a CP to check you off.**

In the `part2` folder, we have provided an implementation of ArrayList for you - there is a header file `arraylist.h`, with the specfication of the expected function in the comments, and a pre-compiled the ArrayList implementation. You do not have access to the source code. We do this because again, we do not want you to think about test cases in terms of the source, but in a wider perspective of "how things should work".

There are **three** bugs in the source code, and it is your task to find  the problems by writing a good suite of test. Write a good set of tests for each of `add()`, `insert()`, `set()`, and `remove()` according to the specification provided in the header file, and thinking about edge cases. Find two of these bugs and you're good to go. 

We have written a sample test case for you for the `get()` function, although it is by no means an exhaustive test case. There is also a simple Makefile included for you, simply compile and run the test with `make tests`.

Some tips:

- [Google Test documentation](https://code.google.com/p/googletest/wiki/V1_7_Primer) has a lot more macros you can use: `EXPECT_NE`, `EXPECT_LT`, `EXPECT_TRUE` etc.
- When you call a method, how should the state of the object change?
- What are the "boundaries" of this class? What happens there?
- What are the execution paths in each function?

## Appendix: A little bit more on the Makefile

You might be a little more confused about what's happening, and how it works. This section can perhaps give you a little bit more idea. Included in the `part1` folder is a simple Makefile. There are several key parts to understand here:

**Automatic Variables** - Things that look like `$@`, `$<`, and `$^` are called automatic variables. When Make parses through the Makefile, it'll automatically substitute them with the correct string. 

- `$@`: target name
- `$<`: first dependency
- `$^`: entire dependency list. 

**`GTEST_LL`** - This is variable that contains the necessary flags to compile a Google Test program. There are several flags in here:

- `-I /usr/local/opt/gtest/include/`: `-I` means "look up includes in this directory". This is how `#include "gtest/gtest.h"` worked in the test program - when C++ tries to look up a file, it looks not only in the current directory, but in whatever directories specified by `-I` as well. Including the gtest header ensures all necessary functions are imported.
- `-l gtest`: Use the library called "gtest". This includes all extra dependencies not included in the header file
- `-l gtest_main`: Use the library called "gtest_main". Notice that there are no main functions at all in our test suite program. THat's because the `gtest_main` library comes with one. When we run the test, we're running a main function included in the Google Test framework that automatically detects all tests and execute them accordingly.
- `-pthread`: Because Google Test runs tests in parallelize, enable threading support

You can probably safely copy this variable everywhere.

**bin/fib.o** - This is the rule to compile the Fibonacci class by itself. Notice that the `-c` flag is used - when we compile the class by itself, we can include the compiled object everywhere - in an actual program, or in the test suite. 

**bin/fibTest** - The main test rule. It has two dependencies: the compiled fib object, and the test suite itself. For the command, we simply take all the dependencies and compile them, with the `GTEST_LL` variable. It's important that the libraries are loaded after the source files, or else the linker will likely throw an error.

**tests** - A rule that just runs the tests. Optional. Notice that this is a phony rule, because it doesn't actually create any file.



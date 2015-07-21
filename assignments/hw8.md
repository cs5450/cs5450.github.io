---
layout: default
title: Homework 8
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
Build your own m-ary `Heap` class whose public definition and skeleton code are provided in the `hw8` folder of the `homework-resources` with the interface given below.  Rather than specifying a specific type of heap (Min- or Max-Heap) we will pass in a priority Comparator (`PComp`) object so that if the comparator object functor implements a **less-than** check then we will have a min-heap.  If the PComp object functor implements a **greater-than** check we will have a max-heap.  For the `updateKey` operation we will also need to find items in the heap which may require a different comparison technique.  For this we will pass in a "key comparator" (`KComp`).  This should implement a functor (`operator()`) that performs less-than comparison to determine if one `T` item is less-than another by examining whatever "key" data (data that makes it unique) as opposed to comparing "priority" data.

```
template <typename T, typename PComp, typename KComp >
class Heap
{
 public:
   /// Constructs an m-ary heap for any m >= 2
   ///   and allows for user-defined comparators to be passed in
   ///   or default comparators of PComp and KComp type to be instantiated
   Heap(int m, PComp c = PComp(), KComp k = KComp());

   // other stuff
   
};
```

You may use the STL `vector<T>` container if you wish.  You should of course **NOT** use the STL `priority_queue` class or `make_heap`, `push_heap`, etc. algorithms.

You should test your heap either using gtest or another test driver program.  Be sure it works as a min-heap and as a max-heap using different comparators.  For reference if your type `T` has a `<` operator, then C++ defines a `less` functor which will compare the type `T` items using the `operator<()`. Similar there is a `greater` functor already defined by C++ that will compare using the `operator>()`.  They are defined in the `functional` header (`#include <functional>` and you can look up their documentation online for further information.  This is meant to just save you time writing functors for types that can easily be compared using built in comparison operators.

In addition to the normal heap operations you should also implement an `updateKey(const T& old_value, const T& new_value)` operation on your heap.  It should find `old_value` currently in the heap (using `KComp`) and replace it with `new_value`.  It should then promote the `new_value` into the appropriate location in the heap.  We assume that `new_value` is "better" (i.e. smaller for min-heaps or larger for max-heaps than the `old_value`...if that is not the case you can jsut exit the function immediately). This operation is allowed to take O(n) to make your life easy.  Though realize with a hash-table you could implement this in O(log n).  

### Step 2 (Hashtags - 30%)

You will now update your microblog engine and GUI to support a "trending topics" feature to compute the **5** most popular hash tags either over all the tweets.  Add widgets to your main GUI window (or create a new window) that will display the trending topics.  It should be initialized on startup using the hashtag topics from the initial database.  You should provide a "Refresh" button which will update the topics based on new tweets added (i.e. additional hashtag terms being used) when pushed. 

In the new window, display the trending topics using a listbox in your GUI that shows the most popular hash tags in descending order of number of occurrences (also show the number of occurrences for easy testing purposes).  You **MUST** use your heap (choose a value of m and give it an appropriate comparator) to solve this problem.  In particular, you should insert all the hashtags from the database into your heap at startup using the number of occurrences as the priority.  As new tweets are added you will need to update the values in the heap. 

### Step 3 (Users and passwords - 30%)

We will now add support for user logins.

####The Login Window

When the program starts, the first thing the user should see is a login window with two text fields, requesting a username and password.  There should be at least three buttons: 

- Login, which will move the user to the main window of your microblog system, assuming an appropriate username and password has been entered.
- Quit, which will close the program.
- Register, which will use the information in the text fields to create a new user with the corresponding username and password.  The password cannot be longer than 8 characters.
	- If the username already exists, you should display an error message.
    - Otherwise create this user and transition to the main window (i.e. a successful login for this new user).

####Storing Password Information

You will store the password immediately after the username in the user line of the database.  So it should be:

`username password following_username ... following_username`

You will need to update the parsing code in `twiteng.cpp` to read in the password.

However, you cannot store the password as plain text.  You should encrypt the password through use of a hash function, and store the encrypted password.

Since the passwords are encrypted in the sample input we provide you, you can't figure out what password you're supposed to enter!  Here are the passwords for the input file:

	Mark = abc123
	Jill = password
	Tommy = fighton
    Sam = Cs4

Note: Passwords should be limited to 8 ASCII characters at most.  You may assume however that we will only enter 8 characters at most and don't have to error check this.

#####Hash Function

1. First translate the password to a `long long` (you will need this much space!).  If you cast a character as an integer, you will get a value between `0` and `127`.  If the password consists of characters `p1` through `p8`, where `p1` is the leading character, you can translate the password using the following formula:

	```
	p8 + (128 * p7) + (128^2 * p6) + ... + (128^7 * p1)
	```

Depending on how you perform this operation you will need each of the intermediate terms to be a `long long`. Be sure you cast in the right place (i.e. on the right-hand side of your computation).  See the notes at the end of the assignment for further details.

Realize that if the password is less than 8 characters (say only 3 characters) then the formula would be:

	```
	p3 + (128 * p2) + (128^2 * p1)
	```

Hint: In C++ `^` is the bitwise-XOR operator but here we want exponentiation.

2. You will now convert your current `long long` number to base-65521.  Mod your number by 65521 to get the least significant digit.  Divide the original number by 65521, and mod the result by 65521 to get the next digit.  Repeat this until you are finished. This will produce no more than 4 digits `w1 w2 w3 w4`, where `w1` is the most significant digit.  

	Store these values in an unsigned int array (each digit base-65521 can fit in a 32-bit unsigned int).  Place zeros in the leading positions if the password was short enough to only require 3 or less digits
    
3. We will now encrypt the password.  Use the following formula to produce the encrypted result.
	
	```
	(45912 * w1 + 35511 * w2 + 65169 * w3 + 4625 * w4) % 65521
	```

We are supplying you with the 4 random numbers above for testing purposes.  Normally you would generate these numbers yourself.

#####Verifying Passwords

To check whether a user enters the correct password, hash their password and make sure it matches what is recorded in the database.  If it does not match display a dialog box with some kind of error message and once the user clicks OK, just wait for them to enter the username and password again.

####Dumping
Update your code to save any new users as well as the correct hashed-password to the database file.

### Extra Credit Step (Splay trees - 50%)
To earn an additional 50% of a project you can do this problem.  You will max out at 100% of your total project grade (meaning if you already have perfect scores on all your projects, this won't help you.)

Create a `map` implemented as a splay tree.  Create a `SplayTree` class.  You can likely derive from `bst.h` that you received in the previous homework.  It should support the necessary `map` operations of `insert`, `find`, `get`/`operator[]`, and iterators. You **MUST** use the `splay_tree.h` in the `homework-resources` folder as a starting point. Then replace **ALL** the C++ map data members you are using in your twitter engine object to use your new SplayTree maps.  

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw8` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw8 folder `$ cd hw_username/hw8`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.




  + Due: Feb. 13, 2015, 11:59pm (PST)  
  + Directory name for this homework (case sensitive): `hw3`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile and run the code for your tests/programs in problems 2, 3, and 4.  See instructions in each problem for specific rules.
	
###Skeleton Code
Some skeleton code has been provided for you for problem 3 and has been pushed to the Github repository [`homework-resources`](https://github.com/usc-csci104-spring2015/homework-resources ). If you already have this repository locally cloned, just perform a `git pull`.  Otherwise you'll need to clone it.

```
$ git clone git@github.com:usc-csci104-spring2015/homework-resources
```

###Problem 1 (Review Material)
Carefully review Operator Overloading, Copy Constructors, and C++ STL (Interludes 5 and 7 and Appendix I)

###Problem 2 (Copy Constructors and Assignment Operators, 20%)
Copy your `LListDbl` class (.h and .cpp files) from your `hw2` folder to your `hw3` folder.  Modify the linked list to now store integers rather than doubles.  In addition, add the following `public` member functions:

```
  /**
   * Adds an item to the back of the list in O(1) time
   */
  void push_back(const int& val);
  
  /**
   * Copy constructor
   */
  LListInt(const LListInt& other);

  /**
   * Assignment Operator
   */
  LListInt& operator=(const LListInt& other);
```

You should write a test program (using your knowledge of unit-testing) that ensures each of these functions work and also use `valgrind` to check that there are no memory leaks.  Add a rule `copytest` to your `Makefile` to compile all the needed code and your test program.  We should be able to compile your program by simply typing `make copytest`.

### Problem 3 (Sets and Operator Overloading, 40%)

#### Overview 
Write a set class that can store integers.  We have provided a `setint.h` that you should use in the `homework-resources` repository.  If you update (git pull) on that repo which you should already have, you should find a `hw3` folder with the header file in it.  Copy it into your own hw_username/hw3 folder.  

#### Basic Implementation (10%)
To implement the set, make its primary data member your `LListInt` class that you completed in the previous problem (be sure to add necessary #include statements as well). Many `set` functions can be easily implemented by simply utilizing the list functionality you wrote in `LListInt` (i.e. calling member functions of the list data member).  But recall a set cannot contain duplicate data values.

Start by implementing the basic functionality of `empty`, `size`, `insert`, `remove`, and `exists`.  Adhere to a few notes:

1. If a client attempts to insert a value that already exists, the call to `insert` should have no effect
1. If a client attempts to remove a value that does not exist, the call to `remove` should have no effect

Think carefully if you need to add a copy constructor and assignment operator.  Remember the rule of 3 [if you need your destructor to actually deallocate memory, then you likely also need a copy constructor and assignment operator].  If you believe your class should have an explicit copy constructor and assignment operator, add it now.

#### Iteration Ability Implementation (15%)
Add a simple iteration capability to your set class to iterate over the internally stored data.  You should do this by implementing two functions:  `first` and `next`.  `first` should return a pointer to the 1st data item in the set (the pointer should be constant since you shouldn't be able to change the data with that pointer) and do any setup to allow future calls to `next` to return pointers to each subsequent data item in the set.  If the set is empty, `first` should return NULL.  As `next` is called it should return pointers to the 2nd, 3rd, etc. items in the set in sequence.  When `next` is called and no more items are left to iterate over, return NULL.

```
  /**
   * Return a pointer to the first item
   *  and support future calls to next()
   */
  int const* first();

  /**
   * Return a pointer to the next item
   *  after the previous call to next
   *  and NULL if you reach the end
   */
  int const* next();
```

Hint:  Use the functionality of the list and some private data member in your set class to track where you are in the "iteration" process.

**Note**:  As a requirement for the client, if they modify (insert or remove from) the set in between calls to `first` and `next` or successive calls to `next` the iteration process may be corrupted.  Thus any modification to the set should require the client to reinitialize the iteration process by calling `first` again and starting over. In other words, if someone modifies the set during iteration and keeps calling `next`, your codes behavior may be undefined (i.e. it's not your fault if it crashes!!!).  But if they don't modify the set during iteration or call `first` again after modification, your code should work without fault. 

#### Union, Intersection, and Operator Overloading (15%)
Now add member functions to compute the set union and intersection along with appropriate operator overloads ( `|` = union and `&` = intersection ).  These operations should result in a new (though not dynamically allocated) set being returned.  You should be able to invoke these operations either with their function call or the operators as shown below:

```
SetInt s1, s2, s3;
// operations to insert values to s1 and s2

// These two should behave equivalently
s3 = s1.setUnion(s2);
s3 = s1 | s2;  

// As should these
s3 = s1.setIntersection(s2);
s3 = s1 & s2;  
```

Write some test code (using good unit testing principles) to ensure your `SetInt` functionality is working.  Add a rule `settest` to your `Makefile` to compile all the needed code and your test program.  We should be able to compile your program by simply typing `make settest`.

###Problem 4 (STL Maps, 40%)
Write a program to read in student names and their majors and then allows queries as to how many students are declared as certain major, the names of students with a certain major, or the names of students with a certain double-major combination.

TBA



Write a program to read in all the words of a text file that is logically broken into "pages" via a special string `<pagebreak>` and then compute an index that associates each word in the text file (other than `<pagebreak>`) with the page numbers it appears on.  So for example, if the input file provided was:

Sample `pages.txt` input file
```
The quick brown fox jumped over the moon. <pagebreak> Calvin said, 
"I love the 104 class I'm taking."
<pagebreak>
Bye
```

should result in an internal memory index being built that contains:

```
the => {1,2}
quick => {1}
brown => {1}
fox => {1}
jumped => {1}
over => {1}
moon => {1}
calvin => {2}
said => {2}
104 => {2}
love => {2}
class => {2}
taking => {2}
bye => {3}
```

Notice a few things:

1. Anytime you extract a string that equals `<pagebreak>` you should increment the page number on which we assume the following words are written.  **Start page numbering at 1, not 0.** 
1. If a word appears on a page multiple times, that page number should still only appear once for that word
1. Punctuation at the beginning or end of a word should be discarded
1. The index should be case-insensitive (i.e. The, the, and THE are all the same word) and should be converted to all lowercase.
1. Valid words must contain 2 or more letters.
1. Words with punctuation in the middle of the word should be ignored (e.g. I'm) with the exception of the hyphen, `-`, which should cause the word to be split into multiple words.  For example, `first-class` should be split into `first` and `class` while `devil-may-care` should be split into `devil`, `may`, and `care`.

You must use the C++ `map` class to store and build the index.  You may not use any other STL classes like `set`, `vector`, `list`, or `deque` but should instead use classes that you've written in this or previous assignments.

Your program will receive as command-line input the filename of the "pages text", an output filename to create, and then any number of other words which should be searched for once the index is built.  In the example above an execution of your program such as:

`$ ./pgindex pages.txt output.txt The Fox onion 104`

should result in the contents of `output.txt` shown below:

```
1 2
1
None
2
```

Here each search term on the command line results in the pages where that term appears being listed on a single line separated by spaces.  If a search term does not appear in the input file output `None` on the corresponding line.  Search terms should also be dealt with in a case-insensitive manner so that they can match your index.

Hint: The `cctype` library has functions such as `ispunct()`, `tolower()`, etc.

Add a rule `pgindex` to your `Makefile` to compile all the needed code.  We should be able to compile your program by simply typing `make pgindex`.


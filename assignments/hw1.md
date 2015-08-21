---
layout: default
title: Homework 1
nav: assignments
---
## HW1

+ Due: Fri. Sept. 4, 2015, 11:59pm (PST)
+ Directory name in your github repository for this homework (case sensitive): `hw1`
   - Once you have cloned your `hw_usc-username` repo, create this `hw1` folder underneath it (i.e. `hw_usc-username/hw1`)
   - If your `hw_usc-username` repo has not been created yet, please do your work in a separate folder and you can copy over relevant files before submitting
   
###A Few Notes on Repositories

1. Never clone one repo into another.  If you have a folder `cs104` on your VM and you clone your personal repo `hw_usc-username` under it (i.e. `cs104/hw_usc-username`) then whenever you want to clone some other repo, you need to do it back up in the `cs104` folder or other location, NOT in the `hw_usc-username` folder.
1. Your repos may not be ready immediately but be sure to create your GitHub account and fill out the GitHub information form linked to at the end of [Lab 01]({site.url}/labs/lab01.html).

###Skeleton Code

On many occasions we will want to distribute skeleton code, tests, and other pertinent files. To do this we have made a separate repository, [`homework-resources`]({{site.data.main.github}} ), under our class GitHub site.  You should clone this repository to your laptop and do a `git pull` regularly to check for updates.  

```
$ git clone git@github.com:usc-csci104-spring2015/homework-resources
```

Again, be sure you don't clone this repo into your `hw_usc-username` repo but at some higher up point like in a `cs104` folder on your laptop.

###Problem 1 (Course Policies, 10%)
Carefully study the information on the [course web site]({{site.url}}), then answer the following questions about course policies:

**Place your answers to this question in a file name `hw1.txt`**

####Part (a): 
Which of the following are acceptable behaviors in solving homeworks/projects?

1. Looking up information relevant to the course online.
1. Looking up or asking for sample solutions online.
1. Talking to my classmates about the problems.
1. Copying code from my classmates, and then editing it significantly.
1. Asking the course staff for help.
1. Sitting next to my classmate and coding together as a team or with significant conversation about approach.
1. Sharing my code with a classmate, if he/she just wants to read over it and learn from it

####Part (b): 
Which of the following are recommended ways of writing code?

1. gedit
1. emacs
1. Eclipse
1. vim
1. Microsoft Visual Studio
1. notepad

####Part(c): 
What is the late submission policy?

1. One hour late submission is acceptable for each assignment.
1. Each assignment can be submitted up to three days late for 50% credit.
1. Each student has 4 late days of which only 2 can be used per HW
1. Students need to get an approval before submitting an assignment late.

####Part(d): 
After making a late submission by pushing your code to Gihub you should...

1. Do nothing! Sit back and enjoy.
1. Complete the online late submission form
1. Start the next HW sooner

####Part (e): 
Is there a grace period to submit assignments?

1. No
1. There is an hour grace period per assignment for 50% credit.
1. Yes, but only if there is a technical difficulty with submission.
1. There is a day grace period upon approval.



###Problem 2 (Git, 10%)
Carefully review and implement the steps discussed in [Lab1](http://bits.usc.edu/cs104/lab01/). Then, answer the following questions:

**Continue your answers to this question in the file name `hw1.txt`**

####Part (a): 
Which of the following git user interfaces are accepted and supported in this course?

1. Git Bash (Windows)
1. GitHub Desktop Client
1. Terminal (Mac or Linux)
1. Eclipse eGit
1. Tower Git Client

####Part (b): 
Provide the appropriate git command to perform the following operations:

1. Stage an untracked file to be committed. The file is called 'hw1q2b.cpp'.
1. Display the details of the last three commits in the repository.

####Part (c) 
Let's say you staged three files to be committed. Then, you ran the following command: 

`git commit`

What will git do?


###Problem 3 (Review Material, and Programming Advice)
Carefully review recursion and dynamic memory management from your CSCI 103 notes and textbook. You may also find Chapters 2 and 5 from the textbook, Chapters 2 and 3 from the lecture notes, and the C++ Interlude 2, helpful.

You will lose points if you have memory leaks, so be sure to run `valgrind` once you think your code is working.

`$ valgrind --tool=memcheck --leak-check=yes ./sum_pairs input.txt output.txt`

**Hint**: In order to read parameters as command line arguments in C++, you need to use a slightly different syntax for your `main` function: `int main (int argc, char * argv[])`. Here, `argc` is the total number of arguments that the program was given, and `argv` is an array of strings, the parameters the program was passed. `argv[0]` is always the name of your program, and `argv[1]` is the first argument. The operating system will assign the values of `argc` and `argv`, and you can just access them inside your program.

###Problem 4 (Recursion, Dynamic Memory, 25%)

Write a **recursive** function to split the elements of a singly-linked list into two sublists, one containing the elements smaller than a given number, the other containing the elements larger than the number. At the same time, the original list should be **deleted**. Your function must be recursive - you will get only very little credit for an iterative solution.

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

###Problem 5 (Parsing 30%)
Markdown is the language used inside `git` for documentation, and the language your course staff use to maintain your course web page. (It is then used translated to HTML.) We will also use Markdown as a format for "web pages" when you write your own version of Google this semester. While we will later give you code for parsing web pages, you should at least have an idea of what's going on inside that code, so here, you'll write a simple parser.

The input will be a plain text file, in which only the characters `[`, `]`, `(`, and `)` have special meaning. Words will be separated by anything that is not a letter, including numbers, white space, special characters, etc. Links to other pages are denoted by one of the following:

+ `(link location)` is a link to `link location` that is just displayed as is. For instance, `(www.usc.edu)` would display as `www.usc.edu`.
+ `[anchor text](link location)` is a link to `link location` that is displayed as `anchor text`. So `[USC](www.usc.edu)` would display as `USC`, and when you click on it, you are taken to `www.usc.edu`.
+ `[anchor text]` is not a valid link. In this case, the square brackets are just punctuation characters that should be ignored.

Your task is to write a program parsemd, which reads the name of a Markdown text file at the command line, and outputs, one per line, each **word** in the file in the order in which they appear. You should not output any special characters, numbers, white space, etc. For each link that you encounter, you should output `LINK (destination, anchor text)`, where `destination` is where the link points, and `anchor text` is the anchor text that is displayed. (If none was specified, that's the same as the destination.)

For example, supposed that the input was the following file `input.md`:

```
Writing my own (www.google.com) in   my 3rd 
semester at [USC](www.usc.edu). 
[Parsing] #!@@!  text   is part of [Assignment 1].
```

You would run

```
$ ./parsemd input.txt
```

The output should be

```
Writing
my
own
LINK (www.google.com, www.google.com)
in
my
rd
semester
at
LINK (www.usc.edu, USC)
Parsing
text
is
part
of
Assignment
```

###Problem 6 (Using Recursion to Generate Combinations, 25%)
A palindrome is a string that is the same when read forward and backward (e.g. `racecar`, `mom`, `otto`). It can be recursively defined as `P = {empty string}` or `P = xPx` where x is some character.

Your job is to write a program that will read in a string of characters and an integer size and generate **all possible** palindromes of the **given size or less** using the characters provided as your options.  Each palindrome should be output to a file.

All the input information will come from the command line:


```
$ ./palindrome out.txt abc 4
```

 
The contents of `out.txt` should have the following palindromes:

```

aa
aaaa
baab
caac
bb
abba
bbbb
cbbc
cc
acca
bccb
cccc
a
aaa
bab
cac
b
aba
bbb
cbc
c
aca
bcb
ccc
```

Note: In the above file the first line is the empty palindrome (length 0) which is a valid palindrome.

To do this you must write a function that directly or indirectly (via a helper function) uses recursion to generate these palindromes.  The function signature should take an ostream (by reference), the array of character options to use, and the size of the desired palindromes.  Thus, your function's signature should be:

```c++
// client uses this interface
void makePalindromes(ostream& ofile, char* options, int size);
```

You may feel free to define a helper function such as:

```c++
// recursive helper function
void makePalindromeHelper(ostream& ofile, char* options, int len, int size, string pal);
```

Again, the helper function is optional or you can change its interface if you desire.

A few things to note/remember.  

1. The empty string is a valid palindrome
1. Palindromes can be odd or even in length
1. The characters in `option` will be unique (no duplicates)
1. Recursion usually replaces 1 loop (in this case it will be easiest to make a recursive call to add more letter(s) to the palindrome.
1. Recursive functions can contain loops (in this case it will be easiest to use a normal loop to iterate over each character in the set of options.
1. Strings support the "+" operator to append words (e.g.  `"cat" + " " + "dog"` yields `"cat dog"`)
1. Your palindromes can be output in any order

We have provided a skeleton for you in the `homework_resources/hw1` folder/repo.  Copy the skeleton file `palindrome.cpp` to your `hw_usc-username/hw1` folder and write the code.  


### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw1` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your `hw1` folder `$ cd hw_username/hw1`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.
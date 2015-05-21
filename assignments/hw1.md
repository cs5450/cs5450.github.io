---
layout: default
title: Homework 1
nav: assignments
---
## HW1

+ Due: Fri. May 29, 2015, 11:59pm (PST)
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
1. Sharing my code with a classmate, even if he/she just wants to read over it and learn from it

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
1. Each student has 4 grace days of which only 2 can be used per HW
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



###Problem 2 (Git, 6%)
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


###Problem 3 (Review Material)
Carefully review recursion and dynamic memory management from your CSCI 103 notes and textbook. You may also find Chapters 2 and 5 from the textbook, and the C++ Interlude 2, helpful.


###Problem 4 (Strings and Streams, Dynamic Memory, 19%)
Write a program that reads in a text file with a single integer on each line and then outputs the sum of the first integer and last integer, second, and second to last integer, etc.  If there is an odd number of integers, add the middle one to itself.  The program must get the name of the text file as the first command line argument; do not hard-code the filename, and do not ask the user with `cin` or `scanf`.  Your output must be another text file whose name is given as the next command line argument.

A sample invocation of your program would be:

`$ ./sum_pairs input.txt output.txt`

The first line of the program will contain the number of integers/lines that follow.  Do not sum that value...it is simply there to tell you how integers follow.  There is no pre-defined upper bound on the number of integers, and you should not hard-code one. Nor can you use types such as `vector` or `list` from the STL. In other words, you must use your own dynamically allocated memory to solve this problem. 


The following is a sample input and output of the program.

Sample Input text file: sum_pairs.in1

```
3
147
104
155
```

Corresponding output:

```
302
208
```

Think about any corner cases (rare cases or cases that may cause your code to break) and be sure your program handles them appropriately.

You will lose points if you have memory leaks, so be sure to run `valgrind` once you think your code is working.

`$ valgrind --tool=memcheck --leak-check=yes ./sum_pairs input.txt output.txt`

**Hint**: In order to read the filename as a command line argument in C++, you need to use a slightly different syntax for your `main` function: `int main (int argc, char * argv[])`. Here, `argc` is the total number of arguments that the program was given, and `argv` is an array of strings, the parameters the program was passed. `argv[0]` is always the name of your program, and `argv[1]` is the first argument. 

###Problem 5 (Parsing 25%)
Write a program that reads a text file that mimics a 'tweet'.  All words of a single tweet are on one line of text with users preceded by an '@' and keywords marked by '#'.  Your program should read a file whose name is specified at the command line and output the following:

1. The number of tweets (number of lines in the file not counting empty lines)
1. All the unique users mentioned in the tweets in the file
1. All the unique hashtags mentioned in the file

You **must use STL vector and string** for this task but no other STL data structures like set or map.

A sample invocation of your program might look like the following:

`./tweet_parse tweets.txt`

Input text file:  tweets.txt

```
Hey @student2, isn't #cs104 the best?

@prof when is HW1 due? #sleepinginclass
@student1 you should talk to @prof
```

The output of your program should be:

```
1. Number of tweets=3
2. Unique users
student2,
prof
student1
3. Unique hashtags
cs104
sleepinginclass
```

Users and hashtags should be ouptut in the order that the first one appeared.  Your output should contain the description lines (1. Number of tweets=, 2. Unique users, etc.) as shown above.

Be sure to make up some of your own test cases and test your program.  Feel free to sharing test **input** (not solution code) files with the class via Piazza.

Hint:  The `getline(stream, string)` function [documented here](http://www.cplusplus.com/reference/string/string/getline/) and declared in the <striong> will be helpful, as will
 stringstreams.  Stringstreams can be used to extract pieces of larger text strings.  Consider using `getline(istream, string, char delimeter)` which will read all the characters out of the istream 
 and put them in the given string until it reaches the delimeter (which by default is the newline `\n` character), discarding the delimeter from the stream when encountered.

``` cpp
string chunk1,chunk2;
string bigText = "Emacs is a great editor!!!";
stringstream ss(bigText);
ss >> chunk1;               // chunk1 =  "Emacs"
ss >> chunk2; 	            // chunk2 =  "is"
```

###Problem 6 (Using Recursion to Generate Combinations, 40%)
An English-language sentence contains a subject and a verb. The subject may be preceded by adjectives while the verb may be followed by adverbs.  Consider the problem of taking a set of options for each part of a sentence and generating the various legal sentences.  A legal sentence is of the following form

`The (adjective) subject verb (adverb).`

All sentences start with `The` and the adjective and adverb are optional.  Subject and verb are required.  If the following options were provided for each part of the sentence...

big  *(adjective)*
boy girl  *(subject)*
ran swam *(verb)*
quickly  *(adverb)*

...we would expect the following sentences.

```
The boy ran.
The boy ran quickly.
The boy swam.
The boy swam quickly.
The girl ran.
The girl ran quickly.
The girl swam.
The girl swam quickly.
The big boy ran.
...
```

Your job is to write a program that will read in the word options for each part of speech and then use a **recursive** approach to generate and output **all the possible sentences** to a new text file, as well as indicating how many sentences were generated.

First, write a function to read in the input words.  Each part of speech will be on one line of the file with words separated by spaces.  The first line will contain the adjective(s), the second line the subject(s), the third line the verb(s), and the fourth line the adverb(s).

```
big
boy girl
ran swam
quickly
```

You should store them in a `vector<vector<string> >` data structure.  That is each string is a word. All the words on a single line should be stored in a vector (that vector represents a 'line').  This is the inner vector. Then each line should be stored in a vector (i.e. the outter vector).  Write a function that takes the filename and a reference to an empty vector of vectors, then uses a filestream and stringstreams to read and parse the words and store them appropriately.  Your function's signature should be:

```c++
void readWords(char* filename, vector<vector<string> >& words);
```

Once you have read in the words, write a recursive function to generate all the sentences and output them.  From the client's perspective they just want to pass the words and an output file stream where the sentences should be written. However, to do your recursive job you will likely need to pass other arguments. Thus a helper function should be defined.  Write the following functions with the majority of the work (and all the recursion) happening in the helper function:

```c++
// client interface
void generateSentences(vector<vector<string> >& words, 
                       ofstream& ofile);

// recursive helper function
void genHelper(vector<vector<string> >& words,
	       ofstream& ofile,
	       int currentOption,
	       string sentence,
	       int& numSentences);
```
A few things to remember.  

1. Every sentence starts with `The`
1. Every sentence ends with a period (.).
1. A sentence may only contain one adjectives and/or one adverb or none. 
1. Every sentence will contain a subject and a verb.
1. Recursion usually replaces 1 loop (in this case it will be easiest to make a recursive call to go on to the next part of the sentence.
1. Recursive functions can contain loops (in this case it will be easiest to use a normal loop to iterate over each word for a given part of the sentence).
1. Strings support the "+" operator to append words (e.g.  `"cat" + " " + "dog"` yields `"cat dog"`)
1. You should output the number of sentences generated as the last line of the file (see sample output below).

We have provided a skeleton for you in the `homework_resources/hw1` folder/repo.  Copy the skeleton file `sentences.cpp` and sample file `words1.in` to your `hw_usc-username/hw1` folder and write the code.  Use `words1.in` as a first test which should produce the following output file.  However, you should create some additional tests. Don't worry we won't use too many words for each option in our test as the number of combinations grows quickly.

Running your program as:

`./sentences words1.in words1.out` 

Should yield the following contents for `words1.out`.

```
The boy ran.
The boy ran quickly.
The boy swam.
The boy swam quickly.
The girl ran.
The girl ran quickly.
The girl swam.
The girl swam quickly.
The big boy ran.
The big boy ran quickly.
The big boy swam.
The big boy swam quickly.
The big girl ran.
The big girl ran quickly.
The big girl swam.
The big girl swam quickly.
16 sentences.
```

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw1` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw4 folder `$ cd hw_username/hw1`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.
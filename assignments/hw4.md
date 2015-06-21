---
layout: default
title: HW4
nav: assignments
---

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw4`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile your `twitter` program.  See instructions in each problem for specific rules.
	
###Skeleton Code
Some skeleton code has been provided for you in the `hw4` folder and has been pushed to the Github repository [`homework-resources`](https://github.com/usc-csci104-summer2015/homework-resources/ ). If you already have this repository locally cloned, just perform a `git pull`.  

###Step 0 (Use of Data Structures and STL, 0%)
You may now use the C++ STL version of all data structures (i.e. `vector<int>`, `map<>`, etc.)

###Project Overview
Our project for the semester will be to model a microblog site such as Twitter. As you will see, it will require using quite a lot of the data structures you are learning about. At a high level, a microblog site is based on the following components:

1. Users that follow other users (this forms a graph with users as vertices and follow relationships as directed edges)
1. A timeline for each user (user x) that shows their posts as well as others posts from those that user x follows and eventually posts with appropriate @ mentions
1. Quick lookup/indexing of tagged keywords (hashtags)

For this (first) part of the project, we focus on parsing users and their feeds and searching for tweets based on hashtags.  You will want to keep an eye on making your code well documented and easy to expand, as you will be adding to it later. (That said, you will also be allowed to rewrite your code later.)

For now, while texts may contain `@username` mentions you don't have to do any special processing of these and can simply show that as text in the tweet

###Step 1 (Classes, 20%)

You will begin your quest by building support to handle Users and their tweets.  

Users contain:

+ a username
+ a list of other Users whom they follow
+ a list of other Users following them
+ a list of tweets that user has posted

Each tweet contains:

+ a timestamp in format:  YYYY-MM-DD HH:MM:SS
+ the username of the User who posted the tweet
+ the actual text of the tweet (we won't impose the 140 character limit in this project)
+ a set of hashtagged words (i.e. if the tweet contains `#cs104`, then this set should contain the string `cs104`)

A DateTime class will model a timestamp

The TwitEng class is the main interface to the application. It should store data related to your microblog engine and perform needed operation.  Good design would separate the user interface of this application (described later) and only provide member functions to carry out the desired operations. In this way we can remove the current command line/text interface and exchange it for a GUI interface but still use the operations provided by TwitEng.

Look in your `hw4` folder of the `homework-resources` folder for skeleton files.  Copy them into your `hw_usc-username/hw4` folder.

Read the following requirements and then implement the classes (feel free to add other classes as needed) and a top level `main()` in `twitter.cpp`

###Step 2 (Parsing and Twitter Feeds, 40%)

Your program will take an input file that contains all the information about users and tweets.  The format of the file and an example that illustrates the format are shown below.

**File Format**

```
number_of_users
username following_username ... following_username
...
username following_username ... following_username
timestamp username tweet_text
timestamp username tweet_text
...
timestamp username tweet_text
```

**Illustrative Example (`twitter.dat`)**

```
4
Mark Tommy Jill
Tommy Jill Sam
Jill Sam
Sam Mark Tommy
2015-05-20 12:35:14 Mark #Selma was an excellent movie to remember the struggle for civil rights
2015-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #football
2015-05-20 00:56:34 Jill Why did Chanos change their name #backwardsdrivethru
2015-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football

```

Note: blank line in the tweets section should just be skipped and processing should continue.

Here Mark is following Tommy and Jill; Tommy is following Jill and Sam; and so on...

Part of this assignment will be to produce one output file per user with the filename `username.feed` (e.g. `Mark.feed`, `Tommy.feed`, etc.).  The requirements for your feed output are:

  1. list the username on the first line
  1. Then list all the tweets from the user as well as any **users being followed** (i.e. if this is Mark's feeds then we should show any tweets from Tommy or Jill. 
  1. List the tweets in sorted order from more recent to least recent
  
Thus, the file above should yield the following feeds:

**Mark.feed**

```
Mark
2014-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #football
2014-05-20 00:56:34 Jill Why did Chanos change their name #backwardsdrivethru
2014-05-20 12:35:14 Mark #Selma was an excellent movie to remember the struggle for civil rights 
```

**Tommy.feed**

```
Tommy
2014-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #football
2014-05-20 00:56:34 Jill Why did Chanos change their name #backwardsdrivethru
2014-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football
```

**Jill.feed**

```
Jill
2014-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #football
2014-05-20 00:56:34 Jill Why did Chanos change their name #backwardsdrivethru
2014-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football
```

**Sam.feed**

```
Sam
2014-05-20 12:35:14 Mark #Selma was an excellent movie to remember the struggle for civil rights
2014-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football
```

**Your program must meet this output format or you will lose significant points as this helps our grading scripts!**

You should use the `User::getFeed()` function to get the tweets to list for each User's feed.  This function returns a `vector<Tweet*>`.  To sort its contents you can use the std::sort algorithm provided in the `<algorithm>` library.  Call it like this:

`sort(myfeedvec.begin(), myfeedvec.end(), TweetComp());`

###Step 3 (Hashtag Index and Search, 40%)

We now want to add the ability to search tweets by hashtags.  This search should also be efficient in time [O(log n) where n = number of hashtags used in the system]  (at the cost of some memory storage).  To do this, you should keep an index of each hashtag term used in the entire system with the tweets that match them. 

Note: Hashtags should be case insensitive.  Think of a way to store them such that users searching for a hashtag #fun will match any of #FUN, #Fun, #FuN, etc.

Once the program loads allow the user to search for tweets that match a given set of hashtags or quit and write the feeds.  Support both an `AND` and `OR` search strategy.  In an `AND` search the user may provide any number of hashtags and see which tweets match **ALL** of those hashtags.  An `OR` search should yield any tweet that matches **ANY** of the provided hashtags.

Provide your user a menu system and respond after each command with the desired outputs. 

So when you run the program as:

`$ ./twitter twitter.dat`
 
A sample run should look like this:

```
=====================================
Menu:
  AND term term ...
  OR term term ...
  TWEET date time username tweet_text
  QUIT (and write feed files)
=====================================

Enter command:
OR football selma
3 matches:
2015-05-20 12:35:14 Mark #Selma was an excellent movie to remember the struggle for civil rights
2015-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #football
2015-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football

Enter command:
AND football selma
No matches.

Enter command:
TWEET Mark Treat CS104 right #summerfun

Enter command:
QUIT
```

When the user types QUIT the feed files should be generated an include any new tweets that have been added during the interactive user session.

When a user adds a tweet you can query the computer system for the current data and time and convert it to your `DateTime` object. It's a bit complicated to extract the current day/time in C++ (other languages give a much cleaner library function to do this) but you are welcome to use the code provided below which populates a `tm` struct (you need to `#include <ctime>`) which you can lookup online to find how to extract the fields you need.

```c++
  // Be sure you #include <ctime>
  
  time_t rawtime;
  struct tm * timeinfo;
  
  time (&rawtime);
  timeinfo = localtime(&rawtime);
  
  // Use timeinfo pointer to access fields of the tm struct
```

See [this link](http://www.cplusplus.com/reference/ctime/tm/).


### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw4` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw4 folder `$ cd hw_username/hw4`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.

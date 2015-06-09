---
layout: default
title: HW4
nav: assignments
---

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw4`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile and run the code for your tests/programs in problems 2, 3, 4, and 5.  See instructions in each problem for specific rules.
	
###Skeleton Code
Some skeleton code has been provided for you in the `hw4` folder and has been pushed to the Github repository [`homework-resources`](https://github.com/usc-csci104-summer2015/homework-resources/ ). If you already have this repository locally cloned, just perform a `git pull`.  

###Step 0 (Use of Data Structures and STL, 0%)
In this assignment you should use your LListStr and SetStr classes where you need to store lists of strings or sets of strings.  Copy those files from your previous homework directory to this `hw4` directory.  For all other data structures (lists or sets of objects, ints, etc. or any maps you should use the C++ STL version of the data structure (i.e. `vector<int>`, `map<>`, etc.)

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


To help you get started we require you to have a few of the following classes.

**user.h**

```c++
#ifndef USER_H
#define USER_H

#include <string>
/* Add appropriate includes for your data structures here */

/* Forward Declaration to avoid #include dependencies */
class Tweet;

class User {
 public:
  /**
   * Constructor 
   */
  User(std::string name);

  /**
   * Destructor
   */
  ~User();

  /**
   * Gets the name of the user 
   * 
   * @return name of the user 
   */
  std::string name();

  /**
   * Gets all the followers of this user  
   * 
   * @return Set of Users who follow this user
   */
  set<User*> followers();

  /**
   * Gets all the users whom this user follows  
   * 
   * @return Set of Users whom this user follows
   */
  set<User*> following();

  /**
   * Gets all the tweets this user has posted
   * 
   * @return List of tweets this user has posted
   */
  vector<Tweet*> tweets();

  /**
   * Adds a follower to this users set of followers
   * 
   * @param u User to add as a follower
   */
  void addFollower(User* u);

  /**
   * Adds another user to the set whom this User follows
   * 
   * @param u User that the user will now follow
   */
  void addFollowing(User* u);

  /**
   * Adds the given tweet as a post from this user
   * 
   * @param t new Tweet posted by this user
   */
  void addTweet(Tweet* t);

  /**
   * Produces the list of Tweets that represent this users feed/timeline
   *  It should contain in timestamp order all the tweets from
   *  this user and all the tweets from all the users whom this user follows
   *
   * @return vector of pointers to all the tweets from this user
   *         and those they follow in timestamp order
   */
  vector<Tweet*> getFeed();

 private:

 /* Add appropriate data members here */

};

#endif
```

**tweet.h**

``` c++
#ifndef TWEET_H
#define TWEET_H
#include <iostream>
#include <string>
#include "datetime.h"

/* Forward declaration */
class User;

/**
 * Models a tweet and provide comparison and output operators
 */
class Tweet
{
 public:
  /**
   * Default constructor -- Initializes to current system day/time
   */
  Tweet();

  /**
   * Constructor 
   */
  Tweet(User* user, DateTime& time, std::string& text);

  /**
   * Gets the timestamp of this tweet
   *
   * @return timestamp of the tweet
   */
  DateTime const & time() const;

  /**
   * Gets the actual text of this tweet
   *
   * @return text of the tweet
   */
  std::string const & text() const;

  /**
   * Return true if this Tweet's timestamp is less-than other's
   *
   * @return result of less-than comparison of tweet's timestamp
   */
  bool operator<(const Tweet& other){
    return _time < other._time;
  }

  /**
   * Return true if this Tweet's timestamp is greater-than other's
   *
   * @return result of greater-than comparison of tweet's timestamp
   */
  bool operator>(const Tweet& other){
    return _time > other._time;
  }

  /**
   * Outputs the tweet to the given ostream in format:
   *   YYYY-MM-DD HH::MM::SS username tweet_text
   *
   * @return the ostream passed in as an argument
   */
  friend std::ostream& operator<<(std::ostream& os, const Tweet& t);

  /* Create any other public or private helper functions you deem 
     necessary */
 
 
 private:
  DateTime _time;
  std::string _text;

  /* Add any other data members you need here */

  
};

/* Leave this alone */
struct TweetComp
{
  bool operator()(Tweet* t1, Tweet* t2)
  {
    return (*t1 > *t2);
  }
};
#endif

```

**datetime.h**

``` c++
#ifndef DATETIME_H
#define DATETIME_H
#include <iostream>

/**
 * Models a timestamp in format YYYY-MM-DD HH:MM:SS 
 */
struct DateTime
{
  /**
   * Constructor
   */
  DateTime();

  /**
   * Another constructor 
   */
  DateTime(int hh, int mm, int ss, int year, int month, int day);

  /**
   * Return true if the timestamp is less-than other's
   *
   * @return result of less-than comparison of timestamp
   */
  bool operator<(const DateTime& other);

  /**
   * Return true if the timestamp is greater-than other's
   *
   * @return result of greater-than comparison of timestamp
   */
  bool operator>(const DateTime& other);

  /**
   * Outputs the timestamp to the given ostream in format:
   *   YYYY-MM-DD HH::MM::SS
   *
   * @return the ostream passed in as an argument
   */
  friend std::ostream& operator<<(std::ostream& os, const DateTime& other);

  /* Add data members here -- they can all be public 
   * which is why we've made this a struct */

   
};

#endif

```

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

Your job is to produce one output file per user with the filename `username.feed` (e.g. `Mark.feed`, `Tommy.feed`, etc.).  The feed should list the username on the first line and then all the tweets from the user as well as any users being followed sorted based on timestamp.  Thus, the file above should produce:

So when you run the program as:

`$ ./twitter twitter.dat`

It should produce the following files

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

###Step 3 (Hashtag Index and Search, 30%)

We now want to add the ability to search tweets by hashtags.  This search should also be efficient in time [O(log n) where n = number of hashtags used in the system]  (at the cost of some memory storage).  To do this, you should keep an index of each hashtag term used in the entire system with the tweets that match them. 

Once the program loads allow the user to search for tweets that match a given set of hashtags or quit and write the feeds.  Support both an `AND` and `OR` search strategy.  In an `AND` search the user may provide any number of hashtags and see which tweets match **ALL** of those hashtags.  An `OR` search should yield any tweet that matches **ANY** of the provided hashtags.

Provide your user a menu system and respond after each command with the desired outputs. 
 
```
=====================================
Menu:
  AND hashtag_word hashtag_word ...
  OR hashtag_word hashtag_word ...
  TWEET username text_of_tweet_until_newline
  QUIT (and write feed files)
=============================+=======

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

Be sure to add, commit, and push your code in your `hw3` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw2 folder `$ cd hw_username/hw3`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.

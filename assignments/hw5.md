---
layout: default
title: HW 5
nav: assignments
---

##Project, Part 2

  + Due: See Assignments page  
  + Directory name for this homework (case sensitive): `hw5`
    - This directory should be in your `hw_username` repository
    - This directory needs its own `README.md` file
    - You should provide a `Makefile` to compile your `twitter` program.  See instructions in each problem for specific rules.

###Overview

For this (second) part of the project, we will focus on adding support for @-mentions, while also using Qt to put a nicer user interface on your microblog site. In creating a Qt interface, you will practice various more complex relationships between objects, along with inheritance. We will describe it in more detail below.


### Step 1 (@-Mentions - 25%)
You will now add support for @-mentions, appropriate visibility of tweets starting with @-mentions, and a separate @-mention feed.

Your microblog site should implement the following rules for handling @-mentions...

1. Add support for a separate "mentions" feed (which is separate from the normal feed) for a given user. Like your main feed, this feed should be sorted in timestamp order (from most to least recent)
1. A tweet that contains an @username mention should be added to the @-mention feed of the specified user who is mentioned.  If the @username is not the very first string of the tweet then it should still appear in the feeds of all users following the user who posted the tweet
1. In addition to display on the mentions feed, a tweet that contains an @username mention such that both the poster and mentioned user follow each other should also appear on each persons main feed.
1. A tweet that starts with an @-mention should not be displayed on the feeds of the followers of the poster.  It should still follow the rules listed above for display on the poster's and mentioned user's feeds

 
### Step 2 (Qt Front End - 75%)

You should add a Qt front end to your Twitter interface so that you can select which users' feed to view and also enter new tweets from a given user.

Your Qt interface must include the following features:

1. A list (scrolling, dropdown box, combobox, etc.) of all the users and appropriate buttons (if any) such that you/we can select which user to "be" 
1. A scrollable list of tweets representing the tweets in the user's feed.
1. A separate list for tweets in the mentions feed for the user
1. A textbox and any appropriate buttons to enter a new tweet for the given user which updates all appropriate feeds
1. A way to "follow" another user [i.e. for the current selected user (a.k.a. user1), provide a means to select another user (a.k.a. user2) that user1 wants to follow and update the appropriate data structures so that feeds are updated appropriately 
1. Add a button that allows the user to open a **NEW** window to search for tweets with given hashtag(s) using an AND or OR search approach.  The user should be able to select AND or OR with radio or push buttons, enter terms (separated by spaces) in a text box, and then click search.  Resulting tweets should be displayed in a listbox.  The user should be able to then close the window from view.  The next time they open the search window, the old contents should be removed/cleared (we don't want to see the old search results).   
1. While your program should still automatically initialize the system with data from an input file given at the command line, add a new feature to be able to save all current info (including tweets added during the execution of the program) to a new file.  The name of the file should be specified by the user...you can choose a text box and a push button to save or use a file dialog box (see `QFileDialog::getSaveFileName()`).  The file should be written out in the same format as the input file.

### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw5` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw2 folder `$ cd hw_username/hw5`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.









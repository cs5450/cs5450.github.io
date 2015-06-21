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
1. In addition to display on the mentions feed, a tweet that contains an @username mention such that both the author and mentioned user follow each other should also appear on each persons main feed.
1. A tweet that starts with an @-mention should not be displayed on the feeds of the followers of the author.  It should still follow the rules listed above for display on the author's and mentioned user's feeds.  Essentially this becomes a "private tweet" in terms of feeds and if there are subsequent @user mentions after the first @mention the tweet should NOT appear in the subsequent users @mention feed.


Given the example below:

```
4
Mark Tommy Jill
Tommy Jill Sam
Jill Sam
Sam Mark Tommy
2015-05-20 12:35:14 Mark #Selma was an excellent movie to remember the struggle\
 for civil rights
2015-05-19 12:35:15 Jill Can't wait for football to start #kesslerforheisman #f\
ootball
2015-05-20 00:56:34 Jill Why did Chanos change their name #backwardsdrivethru
2015-05-21 10:30:27 Sam @Mark when is the UEFA championship? #football
```

The tweet from Sam that started with @Mark would:

- Appear in Mark's mention feed but not main feed (since Mark does not follow Sam).  If Mark did follow Sam then it would also appear in Mark's main feed as well as mention feed.
- Appear in Sam's feed since he authored the tweet
- Not appear in Jill's or Tommy's main feed since @Mark was the first word of the tweet.  Had the tweet started with a non-@-mention Jill and Tommy obviously would have it appear in their feed. 

 
### Step 2 (Qt Front End - 75%)

You should add a Qt front end to your Twitter interface so that you can select which users' feed to view and also enter new tweets from a given user.

####Requirements
Your Qt interface must include the following features:

1. A list (scrolling, dropdown box, combobox, etc.) of all the users and appropriate buttons (if any) such that you/we can select which user to "be" 
1. A scrollable list of tweets representing the tweets in the user's main feed.
1. A separate scrollable list for tweets in the mentions feed for the user
1. A textbox and any appropriate buttons to enter a new tweet for the given user which updates all appropriate feeds (again this should use the current system time for the timestamp)
1. Display a list of other users that the current user is following.
1. A way to "follow" another user [i.e. for the current selected user (a.k.a. user1), provide a means to add another user (a.k.a. user2) that user1 wants to follow and update the appropriate data structures so that feeds are updated appropriately immediately after clicking the button
1. Add a button that allows the user to open a **NEW** window to search for tweets with given hashtag(s) using an AND or OR search approach.  The user should be able to select AND or OR with radio or push buttons, enter terms (separated by spaces) in a text box, and then click search.  Resulting tweets should be displayed in a listbox.  The user should be able to then close the window from view.  The next time they open the search window, the old contents should be removed/cleared (we don't want to see the old search results).   
1. While your program should still automatically initialize the system with data from an input file given at the command line, add a new feature to be able to save all current info (including tweets added during the execution of the program) to a new file.  The name of the file should be specified by the user...you can choose a text box and a push button to save the data or use a file dialog box (see `QFileDialog::getSaveFileName()`).  The file should be written out in the same format as the input file.

####QT Notes
A single class can act as multiple windows by simply having several "Widgets" as either data members (or if your class inherits from QWidget or QMainWindow your object itself can be a window.  Any QWidget that is not a "child" of another widget (i.e. added to a layout that is part of another widget) can act as a top-level window. Simply call `show()` and `hide()` to make the window appear and disappear.

Another important tip is to NOT create windows each time you want to open it.  Essentially create it once at startup and just `show()` it when you need it, `hide()` it when you want it to "close", and simply call `show()` again when you want it to reappear.  Before you call `show()` just populate the windows controls with the updated data you want to display.  Don't reallocate and delete a Widget/window/control multiple time.  The bottom line: **It is best to allocate widgets/controls only once at startup and never again**.  

multiwin.h

```c++
#ifndef MULTIWIN_H
#define MULTIWIN_H
#include <QWidget>
#include <QPushButton>
#include <QLabel>

class Multiwin : public QWidget
{
  Q_OBJECT
public:
  Multiwin();
public slots:
  void mainButtonClicked();
  void otherButtonClicked();
private:
  QPushButton* mainButton;
  QWidget* otherWin;
  QPushButton* otherButton;
};
#endif
```

multiwin.cpp

```c++
#include <QVBoxLayout>
#include "multiwin.h"

Multiwin::Multiwin() : QWidget(NULL)
{
  QVBoxLayout* mainLayout = new QVBoxLayout;
  mainButton = new QPushButton("&Open OtherWin");
  mainLayout->addWidget(mainButton);
  setLayout(mainLayout);

  QVBoxLayout* otherLayout = new QVBoxLayout;
  otherWin = new QWidget;
  otherButton = new QPushButton("&Close");
  otherLayout->addWidget(otherButton);
  otherWin->setLayout(otherLayout);
  QObject::connect(mainButton, SIGNAL(clicked()), this, SLOT(mainButtonClicked()
));
  QObject::connect(otherButton, SIGNAL(clicked()), this, SLOT(otherButtonClicked
()));
}

void Multiwin::mainButtonClicked()
{
  otherWin->show();
}
void Multiwin::otherButtonClicked()
{
  otherWin->hide();
}
```

Layouts and widgets that you don't need to access after creation do NOT need to have to be assigned to a data member in the class.  They can just be created using a temporary pointer and added to some other layout/widget.


### Commit then Re-clone your Repository

Be sure to add, commit, and push your code in your `hw5` directory to your `hw_usc-username` repository.  Now double-check what you've committed, by following the directions below (failure to do so may result in point deductions):

1. Go to your home directory: `$ cd ~`
1. Create a `verify` directory: `$ mkdir verify`
1. Go into that directory: `$ cd verify`
1. Clone your hw_username repo: `$ git clone git@github.com:usc-csci104-summer2015/hw_usc-username.git`
1. Go into your hw5 folder `$ cd hw_username/hw5`
1. Recompile and rerun your programs and tests to ensure that what you submitted works.









---
layout: default
title: Submission Instructions
nav: assignments
---

##Submission Instructions
In all programming assignments, a script will automatically clone the _**master branch**_ of all the private assignment repositories (e.g. repos named `hw_usc-username`) by the deadline and use the state of the repository's master branch at that moment as your official submission for that programming assignment.

In other words, for **on-time submissions** you do not need to actively upload or send your code to any service like Blackboard. You do, however, need to make sure **you `push` your code** to the GitHub server by the assignment's specified deadline(s) and the code you intend to be graded is in the master branch of your repo.

###Step 1. Preparing Your Code for Submission
After concluding work on your assignment, you are to take a moment and make sure that your repository is in a good state for submission. This is a three step process and you are to follow all these steps unless otherwise noted in the assignment write-up, corresponding rubric or explicitly instructed by the professors:

Make your code is grading friendly:

  + Create a `README.md` in the directory for each assignment
  + Suppressed **all** debug messages (remove any `cout` statements or other debug output)
  + All assignment files are in the correct directory, as specified in the assignment page
  + Make sure your code compiles with **no warnings**, **no errors** and **no exceptions**
  + If there are any specific actions and/or commands necessary to **compile**, **run**, or **access** any required documentation for your assignment, include it in a `README.md` file in the assignment's directory.
  + Your grader will grade the assignment on a standard workstation (Linux, Mac, or Windows) equipped with g++ 4.8


####Acceptable Document Formats
In many assignments you will be required to submit documentation and/or textual answers. Your documentation should be in the base directory of the assignment.

The following document formats are accepted:

  + Plain text
  + Markdown

No other formats are accepted unless explicitly stated. These include but not limited to Microsoft Word documents (e.g. `.doc`, `.docx`) and Ritch Text Format (RTF) files.

###Step 2. Push your commits to GitHub
After you've verified that your assignment is ready to be submitted, push your code. Run a `git status` on your repository and make sure that there are **no files** listed as:

  + Changes to be committed,
  + Changes not staged for commit, or
  + Untracked files

A git status on your master branch must return:
    
```
# On branch master
nothing to commit, working directory clean
```

###Step 3. Verifying Your Submission _Before_ the Deadline
**Before** the deadline and **after** pushing your submission to GitHub, you **must** ( **MUST** ) clone your code in a separate directory (i.e. new location), e.g.

```bash
cd
git clone git@github.com:usc-csci104-spring2014/dslib_ttrojan.git test_assignment
cd test_assignment
```

Then, **compile**, **run** and **test** your code using the instructions you wrote in your `README.md` file to ensure it works as expected. 

Our reference grading environment is the Virtual Machine we provided for the course. You are encouraged to test your code on the VM to ensure that your code properly works on an environment other than your own development machine.

###Late Submissions
**After completing** your [late] assignment, the following steps _must_ be followed to signal your intention of making a late submission. Emails to Professors, TAs or CPs are not considered as a notice for late submissions. Failure to follow these instructions will result in grading your repository as it was by the official deadline:

  1. Push your code to your GitHub repository
  1. Submit a [Late Submission Request Form](http://bit.ly/cs104late)
    - Include the [SHA Hash](http://www-cs-students.stanford.edu/~blynn/gitmagic/ch08.html#_integrity) of the commit to be graded. You can get the SHA from your repository's commit page as shown in the following screenshot:

![SHA of latest commit]({{ site.url }}/git/img/github_commit-sha.png)



###Submission FAQs
####Q1. How do I Checkout A Specific Commit
In the case you want to checkout a specific version of your code, such as the commit used in grading your first assignment, You first need to locate the SHA of that commit. To checkout commit `d8da410b19cf0a9f5a3003120204a114b8496942` from ttrojan's restaurant repository:

```bash
cd
git clone git@github.com:usc-csci104-spring2014/dslib_ttrojan.git test_assignment
cd test_assignment
git checkout d8da410b19cf0a9f5a3003120204a114b8496942
```

Notice that by checking out a specific commit, you "detach" yourself from the master branch. When you're done verifying the commit, you should point HEAD back to master. You should never work in a 'detached HEAD' state.

```bash
# reset the changes we've made during the 'detached HEAD' state
git stash
# point HEAD back to master
git checkout master
```


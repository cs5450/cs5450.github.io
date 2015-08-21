---
layout: default
title: Grading
nav: grading
---



### Homework Grade Policies
For each assignment, a precise time will be specified (usually at 11:59.59pm) on the due date. Submission must be made correctly via your github account. **After you believe you have submitted, you should always clone your repo into a new folder and make sure everything you think you submitted was cloned into this new folder.  Then compile your code in this new folder and run it to ensure we will also be able to compile and run your code.** 

### Grading Scale
We will guarantee that you will get at least the grade indicated by the following scale.  At the end of the semester, we may decide to lower the scale if the exams were more difficult than intended.
85%+ = A
80%-85% = A-
77-80% = B+
73-77% = B
70-73% = B-
67-70% = C+
63-67% = C
60-63% = C-
57-60% = D+
53-57% = D
50-53% = D-

#### Grading Disputes
We will work hard to post HW scores and feedback within 2 weeks of the homework's due date. Exams will typically be graded within at most a few days of the exam date.

Any disputes with posted grades <strong>must</strong> be raised within 2 weeks of the score posting. You should **create an issue** in your github repo as detailed below. Notice that any regrade request will result in us trying to give the fairest possible grade to you, which could be higher or lower than the one you received originally.

To raise an issue with your exam score, you should come to the office hours of the professor teaching your section. If you cannot make posted office hours, schedule one by e-mail. The TAs will not be allowed to grant regrades on exams.

Since we want to be able to make sure we can address all of your homework-related concerns as easily as possible, please follow the below policy for creating homework regrade requests:
<ol>
	<li>You will receive a grade report for your homework or project on GitHub.</li>
	<li>If you have questions, you should <strong><em>assign</em></strong> your grader to the issue, and then describe your questions in the comments for this issue.</li>
	<li>If the grader and you cannot resolve the issue, the grader will <strong><em>reassign</em></strong> the issue to one of the TAs.</li>
	<li>The TA will then review your homework and make any necessary adjustments, up or down.</li>
</ol>

Final settlement will be, if necessary, decided by the professors.

### Grading Environment
We will grade your assignments using gcc/g++ at the command line in the virtual machine we provide for the course. You are free to use other compilers or IDEs to develop your code, but in the end, it has to work with g++ in the virtual machine. You probably want to test that it does before submitting.

### Assignment Rubric
Each homework assignment generally asks for a set of features to be
implemented in C++. It also usually asks students to specifically
either use or not use some STL classes. Based on these requirements,
each assignment is going to have its own grading rubric. We also have
a general rubric which you need to consider for *all* assignments;
this captures issues that will be common to most of your projects,
like quality of your code. 

####General Rubric
**Coding problems**

- Up to 3% deduction for using global variables inappropriately or extensively in your program.
- Up to 5% deduction if your code compiles with warnings. It depends on the number of warnings and their severity. In order to see the compiler warnings, you need to compile with the -Wall switch. For example g++ hw1.cpp -Wall -g -o hw1. If you believe the warnings are unavoidable document it in your README file.
- Up to 5% if you do not use command line arguments correctly (hard code or use cin).
- Up to 3% deduction for each compiler error depending on the severity of mistake.
- Up to 10% deduction for bad Makefile (once Makefiles are taught).
- Up to 20% deduction for bad coding practices, code organization, indentation, naming, code file and header separation, code documentation, throwing non-exception types, etc.
- Up to 10% deduction for repo organization, garbage files in repo, folder organization, repeated code files, missing data files, missing README file, etc.
- Up to 10% deduction if your program is testable but crashes during execution, leaks memory, or is considerably slow. Depending on the nature of the assignment, you may lose more points if your program is slow or not scalable.
- If one of the features of an assignment is not testable because it has major compile errors, you will likely lose at least half its points in addition to other coding mistakes. For example if we cannot test one of your data structure functions because it is missing the UI section (main function) or it has major compile errors, then the best we can do is to look at your code and see if it would work or not. In this scenario, we can give you at most half the points for that data structure function.
- Similar to the previous rule, if your project is missing a key feature such that testing many other features depends on it, you will likely lose a lot of points. For example, imagine that your assignment is supposed to read data from a file and then process. If you finish the processing part and leave out the file I/O part, you are making it very difficult to test your project. If you want to leave out the file I/O (maybe because you do not have time), change your main function so that it generates some data (like an initial array) just to test the rest of your assignment. This actually makes it easier for yourself to test your assignment as you build. If you want to learn more about this, look up unit testing.

**Multiple choice problems**

 + If you miss 1 on a problem of several possible answers you get 50%
 + If you miss more than 1 on a problem with multiple correct answer you get 0
 + If you miss the 1 right answer (if there's only 1 correct answer) you get 0

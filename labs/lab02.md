---
layout: default
title: Labs
nav: labs
---

<h2> Debugging </h2>
What is Debugging? Debugging is the process by which programmers attempt to find and remove bugs and errors.  Anytime the code does something you don't want and you attempt to fix it, you are debugging.

What is GDB? GDB stands for <em>GNU Debugger. </em>This is a tool to help you <em>find your issues. </em>GDB won't solve them for you, but it does a good job in helping you find issues in your code by letting you trace and retrace.
<h3 id="toc_2">1. Clone the Labs Repository</h3>
Remember how in the last lab we cloned a repository to bring it down to our local machines? We'll do the same here so that you can get Lab 2 on your computer. Open the terminal, find a location you want to place this course's 'Labs/' folder, and run the following commands:
<pre>git clone git@github.com:usc-csci104-summer2015/labs.git 
cd labs/lab2/</pre>
In the lab2 directory, you should have three files, sample.cpp, lab02.cpp, and Readme.md.
<h3></h3>
<h3>2. Compile and Run</h3>
GDB should already be installed on the Course Virtual Machine (VM). To use GDB, we must have an executable with it.  We'll be using sample.cpp. First compile and run:
<pre>g++ -g sample.cpp -o sample</pre>
Notice the '<strong>-g</strong>' command.  This enables debug information for the resulting executable. More on this later. Let's first run the executable:
<pre>./sample</pre>
The program should have encountered a <em>segmentation fault</em>.  This is an error you may see quite often this semester. Simply put, a segmentation fault means the program is accessing memory that it does not have access to.

By looking at the code in the "sample.cpp", it may not be completely obvious as to why the program may issue a seg-fault.  Let's use GDB to help find the issue.
<h3></h3>
<h3>3. How to use GDB</h3>
GDB uses its own shell to help debug.  This means GDB will look similar to that of a terminal (in fact, it is used inside the terminal!) and you will use gdb-specific commands to perform certain operations.
<h5 id="toc_9">Running GDB</h5>
The first step is to get GDB up and running.  Because we want to debug sample.cpp, we will pass our 'sample' executable as an argument.
<pre>gdb sample</pre>
GDB should now be running within the terminal.  Great!  If at any point you lose track and want to restart, quit the GDB program with the <strong><em>quit</em> </strong>command and rerun GDB.
<h5 id="toc_9">Breakpoints</h5>
Before we begin running the executable within GDB, we would like to set a <em>breakpoint</em>. A breakpoint is a term you will hear often when it comes to debugging.  This is essentially a location that GDB will pause the program at (consider location at a line number).  For example, in our 'sample' program, we already know it has seg-faults, so running it through GDB will do nothing for us unless we stop the program before the seg fault actually occurs.

Let's go ahead and do this. To be on the safe side, lets set a breakpoint right at the beginning of the program. Line 22 is the first line in our main function, let's stop the program there.  To use breakpoints, use the format <em><strong>break &lt;line number&gt;</strong>.</em> As such:
<pre>break 22</pre>
The response you receive should be similar to the following:
<pre>Breakpoint 1 at 0x40091b: file sample.cpp, line 22.</pre>
<em>Note</em>: if you received the response "<em>No symbol table is loaded. Use the 'file' command</em>," this means you did not include the '-g' command when you compiled the program. Follow the "Compile and Run" section to compile correctly.
<h5 id="toc_9">Walking Through the Code</h5>
Let's start the debugging process. Begin by running the program:
<pre>run</pre>
The <strong><i>run </i></strong>command tells GDB to begin running the program. It should stop at the first breakpoint, or until the program stops by itself (whatever comes first).

GDB should have printed out what line we are looking at.  It should look something like this:
<pre>Breakpoint 1, main () at sample.cpp:22
22 cout &lt;&lt; "What is the meaning";</pre>
Let's continue slowly by moving to the next line with the <strong><em>next </em></strong>command:
<pre>next</pre>
This brings us to line 23. Many commands in GDB can be shortened by looking at their first letter (for example, <em>next </em>can be abbreviated to <em>n</em>). Because line 23 is just another print statement, let's go to the next line.
<pre>n</pre>
We should now be at the following line:
<pre>cout &lt;&lt; TheTruth () &lt;&lt; endl;</pre>
This line looks interesting, but if we use the <em>next </em>command again, then we will reach the last line of the program.  Well, the <strong><em>step </em></strong>command can help us here. <em>step </em>allows us to enter into the function given on this line, letting us go a level deeper.  In this case, it would step into the "TheTruth()" function.  If we were already at the deepest level, <em>step </em>would function just as <em>next</em>. Let's use <em>step:</em>
<pre>step</pre>
We should now be at line 11, inside of TheTruth().
<h5 id="toc_9">Viewing Current State of the Process</h5>
If we were tracing our steps through a large program, we may need GDB to help us remember where we've been.  We can look at how we reached where we currently are by using the <strong><em>backtrace </em></strong>command. This can also be shortened to <strong><em>bt</em>:</strong>
<pre>bt</pre>
This should have returned:
<pre>#0 TheTruth () at sample.cpp:11
#1 0x000000000040094b in main () at sample.cpp:24</pre>
`#0` tells us which function we are currently in.  We reached `#0` from `#1` (and if there was a larger backtrace, `#1` would have been reached from `#2`, `#2` from `#3`, and so on).

We can also view the current state of our variables by using the <em><strong>print </strong></em>command.  Let's try printing the new variable located in 'TheTruth()'.
<pre>print buffer</pre>
This is the output on my local machine:
<pre>$2 = (void *) 0x7ffff7b6a19e &lt;std::ostream::flush()+30&gt;</pre>
Yours may look a little different.  Because the variable, <em>buffer, </em>is a pointer, it displays an address (and that address may be different on your computer). Because this variable has not been set yet, this value is garbage. After line 11 is executed, it should receive a new valid address. Let's use <em>next </em>to make that happen.
<pre>next</pre>
Let's make sure <em>buffer </em>received a valid address by printing it's value. Here is the output of my terminal beginning from <em>next</em>:
<pre>(gdb) next
14 *(int*)buffer = ( (1 &lt;&lt; 5) + 10 ) ^ 0x04 ^ 0x04;
(gdb) print buffer
$3 = (void *) 0x0</pre>
Oh no! It seems our <em>buffer</em> variable holds the address 0! If you don't know already, 0x0 is an address generally reserved to represent NULL values, implying our <em>buffer</em> is a NULL value!

Our program hasn't seg-faulted yet because we have not attempted using <em>buffer </em>yet. When we execute it on this next line (line 14), we should see it seg-fault.  Try using the <i>next </i>command to see for yourself.

This must mean line 11 must not have initialized correctly. Let's change the line in sample.cpp from:
<pre>buffer = malloc(1 &lt;&lt; 31);</pre>
to:
<pre>buffer = malloc(1 &lt;&lt; 2);</pre>
Quit GDB using <strong><em>quit</em></strong>.  Recompile and run the sample.cpp.  It should no longer seg-fault.

<em>If you're curious as to why this solves the issue, ask a CP or TA. </em>
<h3></h3>
<h3>4. Find the Secret Message</h3>
Your job is to now use GDB to fix the program from seg-faulting in lab02.cpp. We will check you off when you show us the secret message and tell us what you did to obtain that message. Good luck!
<h3></h3>
<h3>Helpful List of GDB Commands</h3>
<ul>
	<li><em>gdb &lt;executable name&gt;</em> : used to run the GDB program</li>
	<li><em>run </em>: runs the specified program within gdb</li>
	<li><em>quit :</em> quits the gdb environment</li>
	<li><em>next</em> : goes to the next line in sequence of the program.</li>
	<li><em>step :</em> attempts to go deeper into the functionality beneath the current line of code (e.g. stepping into a function). Otherwise acts as <em>next.</em></li>
	<li><em>break &lt;line_number&gt; :</em> sets a breakpoint at the specified line number.  One can also break at a function using the function name.</li>
	<li><em>backtrace : </em>prints the function calls made to get to the current state of the program</li>
	<li>NEW! <i>continue : </i>this is a command you did not learn, but can be quite useful.  Instead of stepping line by line, <em>continue </em>runs the program until a breakpoint is reached or until the program terminates.</li>
</ul>
---
layout: default
title: Tools and Links
nav: tools
---

Additional information will be posted here throughout the course as needed.

###Installing the Course Virtual Machine

 - [Click here for a separate page with instructions to install the course VM.]({{ site.url }}/installing-course-vm.html) Note that this includes downloading a 2.5 GB virtual image that may take several hours; try to download it overnight.

###Background Lectures

 - Mark Redekopp put together [several lectures](http://ee.usc.edu/~redekopp/csmodules.html) recapping material that you should have learned in CSCI 103. These may be particularly helpful if you are a transfer student and your intro class was not as comprehensive as CSCI 103.

###Sample Exams

 - [Sample Midterm](http://bits.usc.edu/files/cs104/midterm.pdf) (no solutions)
 - [Sample Final](http://bits.usc.edu/files/cs104/final.pdf) (no solutions)

<h3 id="toc_4">Linux/Unix Reference</h3>
<ul>
	<li><a href="http://www.nbcs.rutgers.edu/%7Eedseries/UNIXcmds.html">Basic Unix commands</a></li>
</ul>
<h3 id="toc_5">Cygwin</h3>
<ul>
	<li><a href="http://www.cygwin.com/">Cygwin</a> is a full-blown Unix-environment for Windows users, providing you with the gcc compiler, debugger, shell, and everything else you need.</li>
</ul>
<h3 id="toc_6">C++ Resources</h3>
<ul>
	<li>Our <a href="http://www-scf.usc.edu/%7Ecsci104/docs/coding-guide.pdf">guide for writing good code</a>, and help to deal with common mistakes.</li>
	<li><a href="http://www.cplusplus.com/reference/">Cplusplus.com</a></li>
	<li><a href="http://www.cppreference.com/wiki/">CPPreference.com</a></li>
</ul>
<h3 id="toc_7">Editors and IDEs</h3>
We strongly advise you against using gedit, notepad, or other primitive editors. Switch to a professional-level environment or editor, such as the following:
<ul>
	<li><a href="http://www.microsoft.com/visualstudio/eng/2013-preview">Microsoft Visual Studio</a>. A commercial development environment that is generally not free, but for which you can get a free license as a USC student.</li>
	<li><a href="http://www.eclipse.org/">Eclipse</a>. A powerful and freely available development environment, which can be used both with Java and C++.</li>
	<li><a href="http://www.gnu.org/software/emacs/">GNU Emacs</a>. A powerful and freely available editor that is very customizable, and comes with a lot of support for writing and editing code. If you use Emacs, you will likely still need to know how to compile at the command line.</li>
	<li><a href="http://www.vim.org/">VIM</a>. Another powerful and freely available editor that is very customizable and has support for writing and editing code. Same caveat as for Emacs.</li>
	<li><a href="http://www.sublimetext.com/2">Sublime Text</a>. An increasingly popular cross-platform editor. Not free; comes with evaluation period.</li>
</ul>
<h3 id="toc_8">GDB Debugger</h3>
<ul>
	<li><a href="http://www.cs.umd.edu/%7Esrhuang/teaching/cmsc212/gdb-tutorial-handout.pdf">UMD GDB Tutorial</a></li>
	<li><a href="http://www.unknownroad.com/rtfm/gdbtut/gdbtoc.html">RMS's gdb Debugger Tutorial</a></li>
	<li><a href="http://www.yolinux.com/TUTORIALS/GDB-Commands.html">YoLinux.com GDB Cheat Sheet</a></li>
</ul>
<h3 id="toc_9">Valgrind</h3>
<ul>
	<li><a href="http://valgrind.org/docs/">Valgrind Documentation</a></li>
	<li><a href="http://valgrind.org/docs/manual/mc-manual.html">Memcheck: a memory error detector</a></li>
	<li><a href="http://cs.ecs.baylor.edu/%7Edonahoo/tools/valgrind/messages.html">Explanation of error messages from Memcheck</a></li>
</ul>
<h3 id="toc_10">Make</h3>
<ul>
	<li><a href="http://mrbook.org/tutorials/make/">Tutorial on Make</a>.</li>
</ul>

### Git

 - [Git Resources]({{ site.url }}/git-resources.html)
 - Common Git issues and fixes
    + **Issue**: I got the following error when I try `git status` or `git commit`, why?:

	    ```bash
        error: object file .git/objects/99/9269851355238be625dc0c0da9b6a7b8f3965f is empty
        error: object file .git/objects/99/9269851355238be625dc0c0da9b6a7b8f3965f is empty
        fatal: loose object 999269851355238be625dc0c0da9b6a7b8f3965f (stored in .git/objects/99/9269851355238be625dc0c0da9b6a7b8f3965f) is corrupt
	    ```

		**Fix**:
		This can happen if you don't shut down the VM properly. You should always choose the "Shut Down..." option from within Ubuntu: Power -> Shut Down. Don't save the machine state, force quit, or let your computer sleep while you have the VM running (while it won't always cause a problem, you may have issues with Git like this one here). You can find a solution on [at this link](http://stackoverflow.com/questions/11706215/how-to-fix-git-error-object-file-is-empty)  I've followed these steps before and it fixed my issue.
		
		You could also re-clone your repo to a new folder and work out of there. If you've made changes to your files, make sure to copy those changes to another file, then recommit your changes from the new directory. 
	
<h3 id="toc_12">Tutoring</h3>
<ul>
	<li><a href="http://viterbi.usc.edu/varc/">VARC</a> offers tutoring for this class.</li>
</ul>
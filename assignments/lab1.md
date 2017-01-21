---
layout: default
title: Lab 1
nav: assignments
---
## Lab 1

### Checkpoint

Link to [**handout**]({{ site.url }}/assignments/p1/handout/handout.pdf) and [**starter package**]({{ site.url }}/assignments/p1/checkpoint1.tar.gz)

Create a select()-based echo server with support for multiple concurrent clients.
Test using our provided cp1_checker.py test script (read that script and understand it too)
Submission is in the form of a tarball (see handout) which must be uploaded to CMS by {{ site.data.main.lab1_deadline }}.
To aid you in programming an echo server, and testing it, we have prepared this [**starter package**]({{ site.url }}/assignments/p1/checkpoint1.tar.gz) for you. This code needs to be modified to use select() as well as adding support for multiple clients at once.

Files we expect to see:

* Makefile - make sure nothing is hard coded specific to your user; should build a 'lisod' file which runs the echo server
* All of your source code - all .c and .h files
* readme.txt - file containing a brief description of your current implementation of lisod
* tests.txt - file containing a brief description of your testing methods for lisod
* vulnerabilities.txt - identify at least one vulnerability in your current implementation

#### Development advice
The project uses Linux socket library. **Note**: MacOS socket library has a different API.

If you are new to the area, we can suggest few ways to setup a development environment:
1. Use the Linux-based OS, like Ubuntu. You can set it up as a VM through VirtualBox or install it natively.
2. You can try to develop on your OS (Windows or Mac), setup a container and develop remotely.

We have prepared a guide to quick startup with [**beej.us/guide/bgnet/output/html/multipage/index.html**](beej.us/guide/bgnet/output/html/multipage/index.html).

#### Resources to use

Please consult with Beej's guide which is essentially helpful for this project. Here is the link:

### Final Submission


#### Autolab Grading Scripts

Link to [**Autolab Scripts**]({{ site.url }}/assignments/p1/autolab_scripts.tar)

#### Instructions

1. Untar the autolab_grading.tar
1. In the directory that is created you should have 2 files. autograder.tar and Makefile
1. Download your Autolab submission that you want to test and copy it to the above created directory
1. Rename your submission tar to handin.tar
1. Run make inside autolab_scripts
1. This should give you your score.

#### Useful resources:

1. [**FAQ**]({{ site.url }}/assignments/p1/P1_FAQ.txt)
2. [**Annotated Excerpted RFC 2616 Text**]({{ site.url }}/assignments/p1/rfc.txt) - This helps you interpret some RFC text and better understand the exact features you should implement
3. [**Lex Yacc Starter Package(For the HTTP Parser)**]({{ site.url }}/assignments/p1/checkpoint2.tar.gz)
4. [**dumper.py**]({{ site.url }}/assignments/p1/dumper.py) - Point a web browser to this Python web server and observe requests
5. [**Static Site**]({{ site.url }}/assignments/p1/liso.static_site.tar.gz) - Demo static website to serve with Liso
6. [**Liso Prototype**]({{ site.url }}/assignments/p1/liso_prototype.py) - Python web server demoing what requests from Liso should look like; not a full solution
7. [**CP2_Test_Code**]({{ site.url }}/assignments/p1/proj1cp2_checker.tar)- Sample test code for CP2

#### Steps

1. Begin with your repository and state of work from Checkpoint 1
2. Create a logging module for your project.
* Handle creation of a log file as specified in the project handout.
* Make API functions for formatted (up to you; we may specify this later) writing to this log file (not thread-safe).
* Make an API function for gracefully closing this log file.
* Expose and use this module/API within the Checkpoints instead of using stderr or stdout.
* Log IP addresses and browser information/settings as coming from connected clients.
* Log all errors encountered to the log file.
3. Create an HTTP 1.1 parser and layer supporting GET, HEAD, and POST as defined in RFC 2616.
* Your server should serve files out of a specific folder as specified in the project handout document.
* You must read and write files from disk in this step, with full error handling (permissions, file does not exist, IO errors, etc.).
* This server should build off of Checkpoint 1. We have provided some design guidelines for overall architecture below.
* This server should also handle errors in a sane way. It should never completely crash (make it as robust as possible). Send proper HTTP 1.1 error codes back to the browser.
* Errors should be reported to a client as HTTP 1.1 error responses. In addition, they should be logged to a file as specified in the project documentation.
* Submission is the same as Checkpoint 1. Tag and upload your repo in a tarball to the corresponding lab on CMS.
* Checkpoint 2 builds on top of the foundation you built in Checkpoint 1. Try architecting these two checkpoints as 'layers' in your design. You have the low-level select() layer from Checkpoint 1 which serves as a blackbox handling the select() system call and associated recv() and send() calls. It should expose an API to Checkpoint 2 that also uses functions provided by Checkpoint 2. That is, Checkpoint 2 also provides functions used by Checkpoint 1 (you are free to design differently, this is only a suggestion). For example, Checkpoint 2 might provide an HTTP 1.1 finite state machine that interprets and parses bytes coming in on buffers from Checkpoint 1. It might also provide special disconnect handlers as needed to cleanup state if a connection closes inside Checkpoint 1 without sending a proper HTTP 1.1 close header option. In addition, Checkpoint 1 might provide functions or a method of passing back bytes in some buffer that Checkpoint 2 wants to be written back to the client. Architect this however you like, you are the software engineer here designing and building a web server based on select().

Files we expect to see in your submission:

* Makefile - make sure nothing is hard coded specific to your user; should build a 'lisod' file which runs the HTTP 1.1 server
* All of your source code - all .c and .h files
* readme.txt - file containing a brief description of your current implementation of lisod
* tests.txt - file containing a brief description of your testing methods for lisod
* vulnerabilities.txt - identify at least one vulnerability in your current implementation

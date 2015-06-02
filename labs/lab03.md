---
layout: default
title: Labs
nav: labs
---

### Lab 3 - Makefiles

[Intro Music](https://www.youtube.com/watch?v=JuYeHPFR3f0)

Make is a program that makes other programs. This is especially useful when 
your programs become large, and recompiling after an edit requires multiple 
steps. Using a `makefile`, we can configure a program to compile simply by 
typing the `make` command into terminal. This lab will teach you how to write a 
basic makefile to be used in assignments from here on out. 

## 1 - Using `make`

When you type `make` into terminal, it will automatically look for a file named 
`makefile` for instructions. Let's start with a makefile for a single cpp file.

In the part 1 folder, you will find a file called `charizard.cpp`. You can 
compile this simply with the following instruction:

```
(1.0.1) g++ -g -Wall charizard.cpp -o charizard
```

But let's use make. 

### 1.1 Syntax and Structure

A basic makefile's structure is the following:

```
target: dependencies
<tab>system command
```

Each of these is called a rule. If you type `make <target>`, the make tool 
searches for the appropriate target. The dependencies are the files that are 
necessary for make to run that target. The system command, which is basically 
what you'd want to type into terminal when the rule is executed. 

In this instance, we want to use the compile line above (1.0.1).

### 1.2 Default Target

But what happens when you just run `make`? Good question! There's a very nice 
default target called 'all' made just for that. Let's have 'all' make the 
charizard file, which will become the name of our target. So our resulting 
`makefile` is as below:

```
all: charizard

charizard: charizard.cpp
    <(1.0.1)>
```

Save this file as `makefile` in the part1 directory and type make. ~\**~ MAGIC ~*\*~

### 1.3 Cleaning Up

Now large projects will eventually generate a lot of binary files. We want to 
keep our workspace relatively clean by writing a rule to delete the generated 
files. This is also helpful when you're having mysterious problems as they 
sometimes go away after you delete and recompile your binaries. 

Here's our clean rule:

```
clean:
    rm charizard
```

Very simple, since we only generate one file. You can add this to the end of 
your makefile. Now clean your directory using the `make clean` rule. 

## 2 - Compiling Multi-File Programs

### 2.1 Object Files

Compiling a multi-file program requires two main steps: compiling each .cpp file
separately, and putting them all together to form the executable. 

Here we introduce a new binary file type: .o files. These are the intermediate 
files we make in preparation to compile the executable. Any file that doesn't 
contain the main function must be compiled into a .o file (but you may also 
compile the main function into a .o file). 

Let's look inside the part 2 folder. 

```
$ cd part2
$ ls *
```

You will find three classes: AttackMove, Battle, and Pokemon. We want to compile 
each of these into their own .o file of the same name.

Since we're going to have a lot of binary files beyond this point, let's make a 
bin directory to hold them all for easy cleanup. 

```
$ mkdir bin
```

To make an object file, we simply need to add the `-c` flag in the compile 
command. Let's compile AttackMove first.

```
(2.1.1) g++ -g -Wall -c src/attackMove.cpp -o bin/attackMove.o
```

Simple as that. Do the same for the other two classes, and we can then compile 
the main.

### 2.2 Putting It All Together

To compile the main, we just have to include all the .o files that we've already
made in the g++ command.

```
(2.2.1) g++ -g -Wall bin/attackMove.o bin/battle.o bin/pokemon.o main.cpp -o bin/lab3
```

**Note:** A .o file is compiled code that doesn't get linked to other code even 
if it calls functions from other classes. We tell the compiler this using the 
`-c` flag. When we want to compile the full executable, we do not want to have 
the `-c` flag in that statement because we want the compiler to link all the 
code together in the final step. If you get a linker error, it's probably 
something to do with the `-c` flag!

And you have your own pokemon battle simulator! Run it like normal using:

```
(2.2.2) ./bin/lab3
```

## 3 - Makefiles for Multi-File Programs

Well that's great and all, but how do we do that in a makefile?

### 3.1 Dependencies 

Let's go back to the basic make rule structure:

```
target: dependencies
    system command
```

We skipped dependencies before, but it's something we want to use now. If a 
target has dependencies, make first checks if those dependencies exist before 
executing the system command associated with that rule. If the dependencies 
don't exist, make will run the rule to make those dependencies if they exist.

Make will also check to see if the dependencies have been updated since the last
make and will only recompile the dependencies that have changed. This can save 
you a lot of time if you make a change and don't want to recompile all the files
in your project.

Fill out the new makefile below:

```
all: bin/lab3

bin/lab3: bin/attackMove.o bin/<???> bin/<???>
    <???>

bin/attackMove.o: lib/attackMove.h src/attackMove.cpp
    g++ -g -Wall -c src/attackMove.cpp -o bin/attackMove.o

bin/<???>: <???>
    <???>

bin/<???>: <???>
    <???>
```

To test if your makefile works, run `make` and try to run the program.

### 3.2 Variables

Prof. Redekopp decides he doesn't like the students compiling to the `bin` 
directory. He instead wants the directory to be named `exe`. You could easily 
find and replace bin with exe, but that's messy and could generate errors. Why 
not take advantage of variables instead?

At the top of your makefile, let's define:

```
BIN_DIR = exe
```

Now let's replace every instance of `bin` with `$(BIN_DIR)`, like so:

```
$(BIN_DIR)/attackMove.o: lib/attackMove.h src/attackMove.cpp
    g++ -g -Wall -c src/attackMove.cpp -o $(BIN_DIR)/attackMove.o
```

Now when the profesor changes his mind again and wants a different name for the 
directory, we can just change the variable at the top. (But in general, keep the 
binaries in a directory named `bin`.)

Another use for this is to have a CPPFLAGS (compiler flags) variable that holds 
all the flags you frequently use to compile. We also want to have a CXX variable
to specify which compiler to use. For example:

```
CXX = g++
CPPFLAGS = -Wall -g

$(BIN_DIR)/attackMove.o: lib/attackMove.h src/attackMove.cpp
    $(CXX) $(CPPFLAGS) -c src/attackMove.cpp -o $(BIN_DIR)/attackMove.o
```

### 3.3 DIRSTAMP

If you tried to make again with the above changes, you'll probably get an error 
that the exe directory doesn't exist. Well that's a pain. 

Let's define a rule to make sure the exe directory exists.

```
$(BIN_DIR)/.dirstamp:
    mkdir -p $(BIN_DIR)
    touch $(BIN_DIR)/.dirstamp
```

The .dirstamp file is a hidden file we make to make sure a directory exists. You
can add it to the dependency list of your compile commands.

```
$(BIN_DIR)/attackMove.o: lib/attackMove.h src/attackMove.cpp $(BIN_DIR)/.dirstamp
    g++ -g -Wall -c src/attackMove.cpp -o $(BIN_DIR)/attackMove.o
```

### 3.4 PHONY and Clean

Remember back when we wrote our clean rule for part 1? Well there's a small 
problem with the original rule. If a file named 'clean' exists in our directory,
make won't run the clean rule because it sees that the file already exists!

To get around this, we declare the clean rule as PHONY to tell make that it's 
not associated with a file.

```
.PHONY: clean
clean:
    rm -rf $(BIN_DIR)
```

Since we put all our executables in BIN_DIR, we can simply delete the whole 
folder. Simple as that!

**Note:** there's a danger when using rm -rf as it will irreversably delete whatever
BIN_DIR is without prompting additional confirmation. Be good and don't delete 
your entire OS. 

## Final Makefile

Your final makefile should look something like this:

```
CXX = g++
CPPFLAGS = -g -Wall
BIN_DIR = bin

all: $(BIN_DIR)/lab3

$(BIN_DIR)/lab3: <???>
    <???>

<???>.o: <???>
    <???>

<???>.o: <???>
    <???>

<???>.o: <???>
    <???>

.PHONY: clean
clean:
    rm -rf $(BIN_DIR)

$(BIN_DIR)/.dirstamp:
    mkdir -p $(BIN_DIR)
    touch $(BIN_DIR)/.dirstamp
```

If you really want, you can even have the makefile run the executable for you.

```
all: $(BIN_DIR)/lab3
    ./$(BIN_DIR)/lab3
```

[Outro Music](https://www.youtube.com/watch?v=9FBNRgO3-30)

## Review Questions

1. What is the purpose of the `-c` flag?

1. What is the advantage of compiling to .o files via makefile compared to 
typing it into terminal or compiling the executable together in one step?

1. Why do you not have to include attackMove.cpp when compiling pokemon.cpp 
into pokemon.o?
---
layout: default
title: Labs
nav: labs
---

### Lab 8 - Exceptions

When implementing a function, a data structure, or any piece of code really, a good software engineer should make sure that the product handles any kind of input smoothly. After all, in the real world, there is nobody to promise "perfect input" like our assignments have been doing so.

When the user give your program invalid input, what do you do? So far, we have been just assuming that this never happens, and if it does happen, print out some error statement and "return nothing". In some cases, however, returning "nothing" does not make sense. Considering this example:

```
// Returns the nth Fibonacci number
int getFibonacci(int n);
```

How should the function behave when we pass in an n < 1? We can define some behavior such as `if n < 0, return -1`, but what does that really mean? Mathematically, it does not make sense at all - -1st Fibonacci number just simply does not exist. Chances are, the caller of this function is doing some awkward checks like `if nth Fibonacci number is -1, then ask the user to input again`. Relying on an extra check like this, not to mention the completely arbitrary number `-1`, is not intuitive.

In light of that, we introduce something called an **"exception"**. With an exception, a function can notify the caller of the function if something is going wrong, and the caller can determine what to do with that error. The proper terminology is: an exception is **thrown** when something is wrong, and **caught** by the function caller to figure the appropraite action to deal with the error.

### Catching Exceptions
Let's talk about what we can do as the **caller** of a potential exception-throwing function.

Go into `lab13/example1` and open up `example1.cpp` in your favorite text editor. As you can see, we are creating a map to match up our favorite disney characters with their best friends. `make` the project.

Oh no! The program crashed after we tried to print out Gaston's friend.  He doesn't exist in our map, so the `std::map` class threw an `std::out_of_range` exception. In order to fix the problem, we need to surround our function call with a `try-catch` block as follows:

```
try {
	std::cout << name << "'s friend is " << disneyMap.at(name);
	std::cout << "." << std::endl;
} catch (std::out_of_range) {
	std::cout << name << " has no friends :(" << std::endl;
}
```

Now, when the map throws the `out_of_range` exception, the program will run the code within the catch block instead of crashing.

Compile and run your program again. It should complete with no problems, and forwards the execution to the `catch` block when necessary. Notice that everything else inside the `try` block did not execute when an exception is thrown.

### Making Custom Exceptions
Now open up the `example2` folder and take a look at `example2.cpp`

Notice that this program is trying to insert the seven dwarfs into a linked list and then print their names to the screen. Compile and run the code.

You should be getting a segfault. Go back to `example2.cpp` to try find the problem. It appears that the person who wrote this script mistakenly thought there were eight dwarfs! Of course, we could modify the for loop to run for only 7 iterations, but that wouldn't be a very interesting lab. Moreover, it's a good idea to have some error checking in our linked list.

Open up your `linkedList.h` and take a look at the `get(int index)` function. It will try to get elements even if they are passed the end of the list. Generally saying - you can `throw ` _anything_ - an int, a string, or anything. However, the standard practice is to throw something that inherits from the `std::exception` class - doing so allows anybody to easily understand how to get information out of your exception. Let's make a custom exception class that will print out an interesting error message.  We can insert the following code into the top of our `LinkedList.h`.  

```
class OutOfBounds : public std::exception {
	private:
		int index;
	public:
		OutOfBounds(int i) {
			index = i;
		}
		std::string what() {
			std::string message = "OutOfBounds:"
			"There is no index " +
				std::to_string(index);
			return message;
		}
};
```

When an instance of the OutOfBounds exception is created, it stores as a member variable the index that the user was trying to access.  This way, we can give more information about what caused the error.

Now go back to your `get` function, and add code that will throw an exception when the user tries to get an element that is out of bounds.

```
if(index >= size) {
	throw OutOfBounds(index);
}
```

Compile and run your code again.

We don't have a segfault anymore, but our program is still crashing.  Let's surround the function with a `try-catch` block.

```
try {
	std::cout << dwarfs.get(i) << std::endl;
} catch (OutOfBounds & o) {
	std::cout << o.what() << std::endl;
}
```

Compile and run the program.  Now, it should iterate through all of the dwarves, and then print a helpful message to the user.

### Practice

**More Pokemons!**

In the main `lab8` folder, I have implemented a Pokedex that will print out what the Pokemon looks like given a Pokedex number.  The code works fine in nominal cases, but will segfault and crash in some other cases. Make the program fool-proof by using `exceptions` and handling them with `try-catch` blocks.

The final program should, under no circumstances, crash or segfault. Hint: You'll need at least 2 `try-catch` blocks and 1 `throw`.

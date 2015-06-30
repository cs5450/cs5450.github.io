---
layout: default
title: Labs
nav: labs
---

### Lab 7 - Inheritance and Polymorphism

[Theme Song](https://www.youtube.com/watch?v=FqlxwAzP5zA)

### 1 - Introduction

Inheritance is a method of reusing code by having an object take the properties of another object while adding its own. This is often used to build complex classes from simple ones and/or to make more specific versions of a class. For example, a rectangle object can inherit from a polygon since it is a more specific definition of a polygon.

You can think of class B inheriting class A as class B copy pastes all of class A's code and then adds a few functions or data members of its own. However since the compiler can do the copy pasting for us, we get a much neater version of the code.

### 1.1 Public, Protected, Private

Consider the following:

```
class Person {
	public:
		Person(string name);
		getName();
		getAge();
	protected:
		dreams_;
	private:
		name_;
		age_;
}

class Student : public Person {
	public:
		Student(string name);
		getStudentID();
	private:
		studentID_;
}

class Agent : private Person {
	public:
		Agent(string codename);
		attemptCommunication();
}
```

A few things are going on here. We have a person class that has a name and an age. Our student inherits from person, so it gets access to the name and age as well as the protected dreams of the person (but we wouldn't want to freely share those.)

The agent also inherits from person, but privately. This means anything interacting with the agent can't get the name, age, dreams, or any information on the agent beyond what they can learn from the attempted communication.

Inheriting also elevates everything in your base class to at least that "security level". So in the above case, the all the Person's data members are made private when Agent inherits privately from Person.

###1.2 Initialization Lists

In the above example, how do we create a student and assign the name correctly? `name_` is a private member of Person, so even the student can't access it. We could always make name_ protected, but let's use initialization lists instead and pass name down to the Person constructor from the Student constructor.

```
Student::Student(string name) : Person(name) {
	// rest of student constructor
}
```

###1.3 Virtual Functions

Let's say we have this code:

```
class Shape {
	getName() {return "Shape";}
}

class Rectangle : public Shape {
	getName() {return "Rectangle";}
}
```

Rectangle overloads the getName() function from Shape when it inherits, so if we ever printed `Rectangle::getName()`, it would print "Rectangle". But what if we use a shape pointer?

```
Shape* rect = new Rectangle;
cout << rect->getName() << endl;
```

Unfortunately, this would print "Shape" inless you make getName() a virtual function. This will allow function overloading to ignore the type of pointer we have and only care about the actual object we've made. It's often important to make the destructor a virtual function, because there's a good chance inherited classes need to overload the destructor to prevent memory leaks.

Virtual functions can still be implemented. In fact, calling Shape::getName() should always return "Shape" even when it's a virtual function. Virtual functions just allow for "smart" overloading when dealing with inheritance.

### 1.4 Abstract Classes

In some cases, a base class will want to avoid implementing a function and force its descendents to write the implementation instead. These classes cannot be instantiated by themselves since they're missing implementation, but are very useful in defining a category of objects that have common functionality.

```
class Vehicle {
	virtual void drive() = 0;
}

class Car : public Vehicle {
	void drive {
		moveWheels();
	}
}
```

Here, we've declared the drive() function in Vehicle to be pure virtual, which means Vehicle is now an abstract class. I will never be able to create a Vehicle, but I can create the classes that inherit from it. In this case, we have the Car class that drives by moving its wheels. Since the Car class finished the implementation of the pure virtual function, it can be instantiated.

## 2 - Making an MMO


###2.1 The Players

We're going to be making a very simple RPG today. There are three player classes: tank, healer, and fighter. To simplify the situation even further, they only engage in PvP, meaning they'll only fight other players.

You must write doAction() for all the classes. We've done tank and the base Player for you (that was easy!). Complete healer and fighter. The healer must restore 75 hp to the target up to the maxHP limit, and the figher must deal 75 damage to the target (you can go below 0).

###2.2 Inventory System

Players also have an inventory of items. Similar to the orders from last lab, we're going to use a map to store these items. Notice that we've inherited privately from map, so outside classes cannot call map functions on our Inventory object. Implement the inventory system given the header file we've provided.

When you're done, the provided test should run using `make tests`. The makefile's already written for you. Please don't alter it.

###2.3 Review Questions

To get checked off, all 3 tests must pass. In addition, you must answer the following questions:

1. What class should we inherit from to make a Stack? How would you implement it?
1. What's the purpose of abstract classes? What is required to make a class abstract?
1. Why do we pass a Player pointer to the doAction() function? What are the limitations of passing this Player pointer instead of a more specific class (ex. Healer, Tank, etc.)?

---
layout: default
title: Labs
nav: labs
---

### Lab 6 – Qt Basics

In this lab, we'll be learning about a library called Qt (pronounced "cute"). It is widely popular, available across many platforms (including iOS and Android!), and offers a very robust frameowrk for Graphical User Interface programs. Sadly, we're not using their latest version (4.8 instead of 5.5), because `Virtualbox` doesn't play nice.

By the end of the lab, we'll have a basic GUI for a Reddit-like app.

**NEW!** - Check out the extended lab to see how we can extend this lab assignment into an actual Reddit client!

### 1 - Our first Qt project

Navigate to `example1` folder, and you should see one single file. That's all we need to start a Qt project.

Let's take a look at what's inside - we can go through this line by line.

The first thing that we do is creating a `QApplication` - every Qt project requires initiating it. We pass the command line arguments into the app, in case the user wants to declare some settings for this Qt application. This must be done before any UI elements are created
```
  QApplication app(argc, argv);
```
Then, we create a `QLabel` - essentially a textbox element in Qt. Its constructor takes in a string, which is the text that will be shown inside the textbox. As responsible software engineers working on our first project with a new piece of technology, we use the string, "Hello world!".
```
  QLabel helloWorldLabel("Hello world!");
```
After we've created an instance of the `QLabel`, we need to make sure to show it through a window by calling `theshow( )` method. This method is actually defined in its parent class, `QWidget`. In fact, every GUI element is a QWidget- more on this in a bit.
```
  helloWorldLabel.show();
```
Last but not least, we call `exec( )` on the `QApplication` that we instantiated a while ago, and watch Qt does its magic.
```
  return app.exec();
```
All of this works because Qt doesn't follow the traditional "sequential" paradigm, where the program reads and follows instructions line by line until the program ends. Instead, Qt applications are what we called Event-Driven Programs. These programs are constantly running (like a command line program in an eternal `while` loop waiting for something to happen); it'll respond to "events" such as a button click, and will stay idle for the rest of the time.

Notice that when we call `exec( )` - Qt will take control of the entire program, which means it should be called only after all "initializations" have happened.
Compiling the project

Alright, let's see what this code gets us.

If you're trying to compile with a simple `g++ main.cpp -o main`, you'll quickly realize that it doesn't work. This is becuase Qt comes with a lot of dependencies (for exmaple, `QApplication` and `QLabel`), and we need to link them correctly. However, this isn't always an easy task as Qt is a huge library, and there can be a lot of dependencies to manage.

Instead, we use a tool called `qmake`. There are 3 steps to compile a Qt application.

  1. Generate a Qt Project File: qmake -project Qt will go through every single file in this folder, and make a platform-indepdent project file called [foldername].pro. The project file is also where you can configure compile settings.
  2. Based on the .pro file, generate a Makefile: qmake Qt will read the contents of the .pro file, and genereate a platform-dependent Makefile.
  3. Using the Makefile, compile: make

You don't actually need to use all three commands every time you compile. Most times, you'll only need to run make. When do you need to run `qmake -project` or `qmake`?

What should you include in your homework repository? As a general rule, do not push any dynamically generated files. The corollary of that rule is that, if you have edited any of these files, you should include them. There is actually quite a bit of room to play around with in the project file (`.pro`) - if you've made changes to that file, include that. If not, don't. You can be sure that the graders will be following these 3 steps to compile your Qt project if you submit without any of these files.

#### Running the project

By default, Qt compiles to a binary file with the same name as your folder name. So in this case, the output is the `example1` file. Run it - you should the words "Hello World" in a window. How exciting!

### 2 - Widgets and Layouts

A Widget is a "thing" that a user can see. They can be buttons, text fields, date picker, etc... they can process user input, and emit "Signals" to let other parts of program know that there might a be an "event" that they care about. We've already talked about `QLabel`, but there are also others like `QPushButton`, `QLineEdit` etc. You might find `theWidget` Gallery useful.

A Layout describes how different widgets are organized and positioned in an user interface. If you want to have multiple widgets showing "together" within one widget (e.g., two buttons in the same window), you'll have to use a layout so that you can specify how to organize their positions.

Widgets and Layouts have interesting relationships: widgets can have layouts, and layouts can contain widgets. Combining many simple layouts together, we can easily create a complex-looking layout.

#### Basic Layouts

Navigate to the `example2` folder, and compile that project (`qmake -project`, `qmake`, `make`). We should be able to see four buttons laid out horizontally across the window.

Open `main.cpp` in that folder, and notice that this looks a little bit more complicated that than first one. Recall that if we want multiple widgets to show up together in the same window, we have to use the same layout. In order to use a layout, we have to have a "container" widget to support it. Therefore, we create a simple `QWidget` and just called it `window`.

We choose the basic horizontal layout `QHBoxLayout` - as we add things to it, the layout will just append them horizontally for us.Then, we proceed to create the 4 buttons as well, and the layout to put it in. One thing to make clear - widgets do not contain other widgets; widgets use layouts that contain other widgets.

Lastly, we need to apply the layout to the container widget that we created before, with `setLayout( )`. Lastly, widgets do not show as a window by itself - we need to call `show( )` on it.

Try it out! Edit the `main.cpp` file and try to see if you can get the buttons to be vertically aligned. The layout you might be looking for is `QVBoxLayout`.

#### Grid Layout

Generally speaking, a combination of `QHBoxLayout` and `QVBoxLayout` is enough to layout most applications. If you're looking something fancy, however, take a look at `QGridLayout`. It works like an old 2003 website, when everyhting is still based in tables. The idea is that, grid layout divides the available space into multiple cells, and you can tell the grid layout where certain widget is (e.g., 1st row, 3rd column) and how much space one widget deserves (e.g., 2 rows, 5 columns).

Navigate to the `example3` folder, and compile. You'll see a nice grid made of `QPushButtons`.

The only difference with implementing `QGridLayout` is in the `addWidget( )` method. Notice how that is used in the `example3` code:
```
layout->addWidget(button1, 0, 0, 1, 3);
layout->addWidget(button2, 1, 0, 1, 2);
layout->addWidget(button3, 1, 2, 1, 1);
layout->addWidget(button4, 2, 0, 1, 3);
```
In addition to taking a `QWidget` pointer, it also takes 4 (optional) arguments: `rowIndex`, `columnIndex`, `rowSpan`, and `columnSpan`. Play around with these values a little bit and create your own layout.

#### Extending Widgets

As you can see, with even the simplest layouts, the `main( )` function can get very cluttered very quickly. One way to make our code look neater is creating our own Widgets. For example, a "user profile widget" might contain several `QLabel`s (name, emails, etc.), and several `QPushButton`s (contact, add friend, etc.) In the `example4` folder, we can see our own widget in action.

Open `profile_widget.h`. Here, we're defining our own class called `ProfileWidget`, which extends from the familiar Qt class `QWidget`. There are several things worth mentioning:

First, notice the `Q_OBJECT` macro at the top of the class. We put it there so that during compile time, Qt will pick it up and inject necessary Qt code in there. The result is stored as a file prefixed by `moc_`. Since they're dynamically generated, we shouldn't push them to the repository.

Second, we are keeping track of every single widget and layout that we have created in the private data variable section. Recall that `addWidget( )` and `setLayout( )` takes in pointers, suggesting that we should dynamically allocate them. We keep a copy of those pointers, so that in the destructors, we can delete every UI element that we have created in order to avoid memory leak.

The constructor of this class takes in 3 string parameters, with which we can use to dynamically generate the `QLabels` - that's exactly what's hapepning in a slightly longer `profile_widget.cpp`.

But the result of all this refactoring is that we have a very clean `main( )` function. In fact - it should be kept that way.

### 3 - Signals and Slots

As we mentioned above, Qt programs are event-driven programs. In Qt, events are represented as "signals", and responders to the signals are called "slots". For example presses a button, that button emits a "clicked" signal, and some other piece of code should "listen" to the clicked signal and act accordingly.

Keep the profile widget header and implementation files open, and let's try to make one of the butons work.

First, identify the signal that we want to listen to. A common one is to listen to. There are "signal events" for almost all kinds of widgets built-in with Qt (and later, we'll learn about implementing signals ourselves). For example, the `QPushButton` has these signals. Let's choose the `clicked( )` signal for the `addFriendButton`.

Then, create a function to call when the signal is fired. In order for a function to be a valid slot, it needs to be under a "slots" section in the header file.
```
private slots:
  void addFriend();
```
In the cpp file, let's create a simple function that opens a dialog:
```
//include at top
#include <QMessageBox>

void ProfileWidget::addFriend() {
    QMessageBox::information(this, "Add Friend", "Friend added!");
}
```
Lastly, we connect the signal that we have chosen and the slot together. We do this through the Qt `connect( )` function, which has the following syntax:
```
connect(EmittingObject*, SIGNAL(signalFunction()), ReceivingObject*, SLOT(slotFunction());
```
The syntax is a little unusual, but we can try to understand it.

 - EmittingObject*: The first argument is the pointer to the object that is emitting an event that you want to capture. For example, this could be messageButton from the profile widget.

 - SIGNAL(signalFunction( )): SIGNAL is a macro that wraps around the function that you want to listen to.

 - ReceivingObject*: The object receiving the signal has to include the macro Q_OBJECT on top of its class definition. Generally, we pass this in, becuase the function we want to call is usually int he same class as the parent widget.

 - SLOT(slotFunction( )): The method that should be called (that belongs to the ReceivingObject when the signal was fired.

For example, if we want to have a function `addFriend( )` called every time the `messageButton` is pressed, we'll include the following line in the constructor:
```
connect(messageButton, SIGNAL(clicked()), this, SLOT(addFriend()));
```
Compile and run it, and try clicking on the button. Voila!

### 4 - Your Turn

Now that you're a guru of Qt, time to build an actual application. We'll build a application that looks like the Reddit front page, displaying a list of posts and allows the user to interact with it. We'll expand on the functionality in the later labs, but for now, let's just stick with layouts and simple signals.

Screen Shot 2015-02-24 at 12.37.35 PM

We have provided several classes for you:

 - Reddit - this serves as the "Server" and "Database" for the application. On instantiation, it creates 3 fake "posts" and stores their pointers inside a vector.

 - Post - a simple struct-like class that has member variables like a Reddit post would. Title, url link, author, etc.

 - MainWindow - derives from `QWidget`, and serves as the main widget/window that everything should be in. We store a pointer to the instance of Reddit here in case we ever want to interact with the database.

  - contains a vector of `PostWidget`'s, in addition to other widgets.

 - PostWidget - derives from `QWidget` as well, and is in charge of rendering one post based on the Post class that it was passed in.

A note about the `.pro` file: Since the c++11 `auto` keyword is used here, we have to compile with `-std=c++11`. This is set in the `QMAKE_CXXFLAGS` option. If you run `qmake -project`, it'll override that file, removing the c++11 setting, and generating compile errors.

What you need to do:

 - Finish the PostWidget class constructor based on a Post. They should be in a `GridLayout` - feel free to be creative about the layout, or just follow the screenshot of the example above.
  - main.cpp is set to just show one `PostWidget`. So if you make you focus on the `PostWidget`, compiling and running should show your changes immediately.
  - When you're done with this part, open `main.cpp` and uncomment the lines appropriately so that the `MainWindow` shows instead.
  - Finish `setupPosts( )`, which takes in a vector of `Post` and create `PostWidget` dynamically
  - add them to the layout
  - add them to the vector `postWidgets`, so that we can delete them later in `clearPosts( )`
  - Finish `clearPosts( )`, which should remove all the `PostWidgets` from the layout (by calling `removeWidget( )`, and delete all the pointers in the vector `postWidgets`
  - Create a new button with the text "quit", and add it to the layout
  - Create a quit function that serves as a slot
  - Connect the button's `clicked( )` to the quit function.
  - Make sure you delete this button in the destructor.

Some tips:

  - If you look at the example layout screenshot - you should notice that there are 3 rows and 3 columns in each `PostWidget`. How can you use `GridLayout` properties to lay them out?

  - When creating a `QLabel`, the constructor takes in a `QString`. You will need to convert std strings and ints into `QString` in order to pass that in. Use the functions `QString::fromStdString` and `QString::number( )`.
```
    titleLabel = new QLabel(QString::fromStdString(p->title));
    karmaLabel = new QLabel(QString::number(p->karma));
```
    To change the text size / boldness, use `QFont`:
```
      QFont titleFont;
      titleFont.setBold(true);
      titleFont.setPointSize(14);
      titleLabel->setFont(titleFont);
```
    You can actually use HTML in `QLabel`. To make a link clickable and open a browser:
```
      url = "http://bits.usc.edu/cs104";
      urlLabel = new QLabel(QString::fromStdString("<a href=\"" + url + "\">" + url + "</a>"));
      urlLabel->setOpenExternalLinks(true);
```
    To quit, run the following:
```
      QApplication::exit();
```
    Qt is very well documented. You might want to check out documentation for `QGridLayout`, `QLabel`, `QPushButton`, etc.

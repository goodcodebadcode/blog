+++
aliases = []
comments = true
date = 2021-02-08T10:04:00Z
description = "Standardise your projects and code using imports, modules, and packages."
disableShare = false
draft = true
hideMeta = false
showToc = false
tags = ["education", "architecture", "python"]
title = "Structuring your Python project"
tocOpen = false
[cover]
alt = ""
caption = "Photo by Lance Anderson  on Unsplash Rock and Roll Hall of Fame"
image = "/uploads/rock-and-roll-hall-of-fame.jpg"
relative = false

+++
Just as code style, API design, and automation are essential for a healthy productive development cycle. Project structure is a crucial part of your application's architecture.

> Martin Fowler describes architecture as: “The highest-breakdown of a system into its parts.”

In practical terms, architecture means code whose logic and dependencies are clear as well as how the files and folders are organised in the filesystem.

## Sample Project Structure

**tl;dr**: This is a structure regularly used by our internal data science teams. The ref. repository is [available on GitHub]().

There are not a lot of complex rules here because Python projects can be simple.

    /apidocs
    /docs
    /lib
    /sample/__init__.py
    /sample/main.py
    /scripts
    /tests
    .editorconfig
    .gitattributes
    .gitignore
    .pylintrc
    CHANGELOG.rst
    CONTRIBUTING.rst
    LICENSE
    READNE.rst
    makefile
    requirements.txt
    setup.py

Structuring your Python projects can often be one of the most overlooked parts of onboarding new developers onto a team. If your repository is a massive dump of files or a nested mess of directories, any new developer or contributor will have difficulty getting to grips with your code, regardless of how beautiful your documentation might be.

In truth, I have been guilty of this too and have seen many developers and teams get this simple step wrong, often stumbling through a jumble of common mistakes until they arrive at something that at least works e.g. playing tricks with `sys.path` is an instant DQ in my book.

* **/apidocs**: Epydoc-generated API docs.
* **/docs**: All your documentation goes here.
* **/lib**: For any C-Language libraries.
* **/scripts** or **/bin**: Any command-line related items.
* **/tests**: For your tests.

The top-level directory contains other related files.

The hardest choice is whether or not to use a `/src` tree. Python doesn't have a distinction between `/src`, `/lib`, and `/bin` like Java or C has. Since a top-level `/src` directory is seen by some as meaningless, your top-level directory can be the top-level architecture of your application, for example `/sample`.

> **Remember**: Your module packages are the core focus of your project. They should not be hidden away. If, however, the module consists of only a single file, place it directly in your the root directory of your project.

I recommend putting all of this under a "name-of-project" directory for example. So, if you're writing an application named `quux`, the directory that contains the structure is named `/quux`. This then means that another project's `PYTHONPATH`, then, can include `/path/to/quux/sample` to reuse the `QUUX.sample` module.

Some of these files will be new to you, so let’s take a quick look at what each of them does.

* **.editorconfig**: Is a file format and collection of text editor plugins for maintaining consistent coding styles between different editors and IDEs.
* .**gitattributes**: A simple text file that gives attributes to pathnames.
* **.gitignore**: Specifies intentionally untracked files that Git should ignore.
* **.pylintrc**: Allows you to tell Pylint to ignore certain checks.
* **CHANGELOG.rst**: A log or record of all notable changes made to a project.
* **CONTRIBUTING.rst**: Create guidelines to communicate how people should contribute to your project.
* **LICENSE**: Describes the license for a project. It’s always a good idea to have one if you’re distributing code. The filename is in all caps by convention.
* **makefile**:
* **README.rst**: This is a [Markdown](https://en.wikipedia.org/wiki/Markdown) (or [reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText)) file for documenting the purpose and usage of your application.
* **requirements.txt**: Defines outside Python dependencies and their versions for your application.
* **setup.py**: Can also be used to define dependencies.

The files and folders included here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum if you plan on using a basic layout like this.

## Modules and Packages

Any Python (`.py`) file is a module, and a collection of modules in a single directory with an `__init__.py` file is a package.

This means that if you have two files in the same folder you can load the definitions and statements from one module for use in the other module.

In short, **modules are named by filenames**, and **packages are named by their directory name**.

Let’s take a look at an example simple project:

    helloworld/
    │
    ├── helloworld/
    │   ├── __init__.py
    │   ├── helloworld.py
    │   └── helpers.py
    │
    ├── tests/
    │   ├── helloworld_tests.py
    │   └── helpers_tests.py
    │
    ├── .editorconfig
    ├── .gitattributes
    ├── .gitignore
    ├── .pylintrc
    ├── CHANGELOG.rst
    ├── CONTRIBUTING.rst
    ├── LICENSE
    ├── makefile
    ├── README.rst
    ├── requirements.txt
    └── setup.py   

1. Create 2 files. The main demo file (main.py) and then a second file (funcs.py) which the main file will then call.
2. In funcs.py create two functions which print a basic console message.

To use the functions from funcs.py, you need to import the functions as long as they are in the same directory. You can then call the functions through the funcs module.

When you simply: import funcs — you are essentially importing everything contained in funcs. But if you only want access to a single function you can use: from funcs import functionName.

### Importing modules by alias

To alias an import so that we do not have reference the full name throughout, we can simply:

### Importing a single function

### Importing everything

We do have the ability to import everything from a module using the asterisk (*), however, this frowned upon.

### **How does Python know where my modules are?**

You have probably noticed that we did not provide a file path for the imported modules, yet somehow Python knew where to find these.

When we import a module, Python checks multiple locations and the locations that it checks are in a list found in sys.path:

We can see the output of locations that Python checks that are specific to my machine. These items are then checked in order. Notice here we can see the directory containing the script that we are currently using - which is why it can import our utilities module. This is why we can always import modules from the same directory.

Next it lists directories located in the python env variable. And then, after that it lists the directory for packages in the standard library, which is why we can import the sys module, then lastly it lists the directory for 3rd party packages.

Now what happens when we want to import modules from another location that is not listed in sys.path

Move collection_utils to another location on your machine and we can then try run our code. You can see that this fails.

We could also append the new location to sys.path by _sys.path.append(‘/some/path’)_ and you can now see that this works. However, this is not a recommended approach.

So instead, we could change the location of the Python env variables. Open our bash_profile and add the following:

export PYTHONPATH=‘\~_/some/path’_

Now restart the terminal.

## Packages

1. Create a directory and include a file: **init**.py: This can be blank but it tells the python interpreter that this directory should be treated as a package.
2. Create a new file: say.py: Add a single function hello and pass a name argument, console print ‘Hello {name}’.
3. Using the python interpreter run the package. You do not have to be within the package directory, instead we will run the package from outside of the directory. First import the package and then invoke the hello function.
4. Repeat the process for creating sub-packages in the main package.
5. Show example code by importing both packages, use an alias or import the specific function.

This process is the same for when you import and use other packages in Python. However, they are not located in the same directory but rather located within your package directory for your python installation.

### **if name** == '**main**'

You will see this a lot in Python.

To start let us print **name** (the result is **main**), so what exactly is going on here. Before Python runs any code, it sets a few special variables. One of these special variables is **name**. So when we run some code is file hello.py, Python behind the scenes is setting **name** to equal **main**.

When we import additional modules we are running the code of the imported module, in effect. Python then sets **name** to the name of the file for the import.

So why put your code into a separate main method. It allows the bye.py to call hello.say directly from within the bye.py file.

## The standard library

Python’s standard library is very extensive, offering a wide range of facilities as indicated by the long table of contents listed below. The library contains built-in modules (written in C) that provide access to system functionality such as file I/O that would otherwise be inaccessible to Python programmers, as well as modules written in Python that provide standardised solutions for many problems that occur in everyday programming.

Some of these modules are explicitly designed to encourage and enhance the portability of Python programs by abstracting away platform-specifics into platform-neutral APIs.

In addition to the standard library, there is a growing collection of several thousand components (from individual programs and modules to packages and entire application development frameworks), available from the [Python Package Index](https://pypi.org/).
+++
aliases = []
comments = true
date = 2021-02-08T10:04:00Z
description = "Standardise your projects and code using imports, modules, and packages."
disableShare = false
draft = true
hideMeta = false
showToc = false
tags = []
title = "Structuring your Python project"
tocOpen = false
[cover]
alt = ""
caption = "Photo by Lance Anderson  on Unsplash Rock and Roll Hall of Fame"
image = "/uploads/rock-and-roll-hall-of-fame.jpg"
relative = false

+++
Just as code style, API design, and automation are essential for a healthy productive development cycle. Project structure is a crucial part of your application's architecture.

By architecture we mean the decisions we make when breaking down our project into its constituent parts, so that it achieves all of the technical and operational requirements so that it best meets its objective. This means considering how we optimise for performance, security, manageability and how to best leverage Python’s features to create clean, effective code. 

In practical terms, architecture means making clean code whose logic and dependencies are clear as well as how the files and folders are organised in the filesystem.

> Martin Fowler describes architecture as: “The highest-breakdown of a system into its parts.”

## Sample Project Structure

**tl;dr**: This is what [Kenneth Reitz recommended in 2013](https://kennethreitz.org/essays/2013/01/27/repository-structure-and-python). This repository is [available on GitHub](https://github.com/kennethreitz/samplemod).

when breaking down a project into parts, so that it meets all of the technical and operational requirements.

When a developer joins a new team, they are left to find their way around the source code of whatever project they happened to be assigned to. Structuring your python projects can often be one of the most overlooked parts of on boarding new developers.

In this post, I want to address the **structure a Python project** so that it is easy to understand, and more importantly reusable.

We will explore import statements, modules, packages, and how to structure your project.

Many developers and teams get this wrong, stumbling through a jumble of common mistakes until they arrive at something that at least works.

As we create more and more classes we need a way to organise our code in such a way to make them available and easy to find.

## Modules

A module is a file containing Python definitions and statements. The file name is the module name with the suffix .py appended. Within a module, the module’s name (as a string) is available as the value of the global variable **name**.

Modules in Python are simply files, if you have two files I the same folder we can load a class from one module for use in the other module.

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
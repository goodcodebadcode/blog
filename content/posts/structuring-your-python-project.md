+++
aliases = []
comments = true
date = 2021-02-08T10:04:00Z
description = "Standardise your projects with modules, and packages."
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
    CHANGELOG.rst
    CONTRIBUTING.rst
    LICENSE
    READNE.rst
    pylintrc
    makefile
    requirements.txt
    setup.py

Structuring your Python projects can often be one of the most overlooked parts of onboarding new developers onto a team. If your repository is a massive dump of files or a nested mess of directories, any new developer or contributor will have difficulty getting to grips with your code, regardless of how beautiful your documentation might be.

In truth, I have been guilty of this too and have seen many developers and teams get this simple step wrong, often stumbling through a jumble of common mistakes until they arrive at something that at least works.

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
* **CHANGELOG.rst**: A log or record of all notable changes made to a project.
* **CONTRIBUTING.rst**: Create guidelines to communicate how people should contribute to your project.
* **LICENSE**: Describes the license for a project. It’s always a good idea to have one if you’re distributing code. The filename is in all caps by convention.
* **pylintrc**: Allows you to tell Pylint to ignore certain checks.
* **makefile**: Keeps your project up to date by rebuilding the out of date parts of your project.
* **README.rst**: This is a [Markdown](https://en.wikipedia.org/wiki/Markdown) (or [reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText)) file for documenting the purpose and usage of your application.
* **requirements.txt**: Defines outside Python dependencies and their versions for your application.
* **setup.py**: Can also be used to define dependencies.

The files and folders included here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum if you plan on using a basic layout like this.

## Modules and Packages

Any Python (`.py`) file is a module, and a collection of modules in a single directory with an `__init__.py` file is a package. This file serves many purposes, but for simplicity sakes, it tells the Python interpreter that this directory is a package directory.

This means that if you have two files in the same folder you can load the definitions and statements from one module for use in the other module.

In short, **modules are named by filenames**, and **packages are named by their directory name**.

Let’s take a look at a simple todo project:

    todo-app/
    ├── docs/
    ├── lib/
    ├── scripts/
    ├── todo/
    │   ├── app.py
    │   ├── common/
    │   │   ├── checks.py
    │   │   ├── constants.py
    │   │   ├── helpers.py
    │   │   ├── __init__.py
    │   │   └── status_enums.py
    │   ├── data/
    │   │   ├── database.py
    │   │   ├── __init__.py
    │   │   ├── tasks.py
    │   │   └── users.py
    │   ├── todos/
    │   │   ├── auth.py
    │   │   ├── __init__.py
    │   │   ├── data_loader.py
    │   │   ├── todo_item.py
    │   │   └── todo_list.py
    │   ├── __init__.py
    │   ├── __main__.py
    │   └── tests/
    │       ├── __init__.py
    │       ├── mock_db.py
    │       ├── mock_db_config.py
    │       ├── test_auth.py
    │       ├── test_checks.py
    │       ├── test_data_loader.py
    │       ├── test_helpers.py
    │       ├── test_tasks.py
    │       ├── test_todo_item.py
    │       ├── test_todo_list.py
    │       └── test_users.py
    ├── .editorconfig
    ├── .gitattributes
    ├── .gitignore
    ├── CHANGELOG.rst
    ├── CONTRIBUTING.rst
    ├── LICENSE
    ├── makefile
    ├── pylintrc
    ├── README.rst
    ├── requirements.txt
    └── setup.py   

## Importing modules

What happens when you have code in one module that needs to access code in another module or package? You import it!

When you import a module, we are actually running it. This means that any recursive or inner `import` statements within the module are also run.

```python
import re
```

So when we import `re.py`, we are in fact importing `re.py` and its own several import statements. That doesn't mean those imports are available to the module we imported `re` from, but it does mean those files have to exist. If, for some unlikely reason, `enum.py` was deleted on your environment, and you ran `import re`, it would fail with an error.

I have often heard newer Python developers ask why the inner module, `enum.py` in our example is being imported, since they didn't ask for it directly in their code. The answer is simple: we imported `re`, and that imports `enum`.

### How does this actually work?

You have probably noticed that we did not provide a file path for the import, yet somehow Python knew where to find these. 

The first thing Python will do is look up the name of the module in [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules). This is a cache of all modules that have been previously imported.

If the name is not found in the module cache, Python will proceed to search through a list of built-in modules. These are modules that come pre-installed with Python and can be found in the [Python Standard Library](https://docs.python.org/3/library/). If the name still isn’t found in the built-in modules, Python then searches for it in a list of directories defined by [`sys.path`](https://docs.python.org/3/library/sys.html#sys.path). This list usually includes the current directory, which is searched first.

When Python finds the module, it binds it to a name in the local scope. This means that import is now defined and can be used in the current module without throwing a `NameError`.

If the name is never found, you will get a `ModuleNotFoundError`. You can find out more about imports in the [Python documentation](https://docs.python.org/3/reference/import.html).

### The standard library

Python’s standard library is very extensive. The library contains built-in modules, written in C, that provide access to system functionality such as file I/O that would otherwise be inaccessible to Python programmers. It also includes other modules written in Python that provide standardised solutions for many problems that occur in everyday programming.

Some of these modules are explicitly designed to encourage and enhance the portability of Python programs by abstracting away platform-specifics into platform-neutral APIs.

## Syntax of Import Statements

There are a number of ways of importing, but there are few written and unwritten rules that you should follow.

For practical purposes, let us assume we have the following two files `main.py` and `bank_account.py` located in the same directory.

    bank_app/
    ├── bank_account.py
    └── main.py

The contents of `bank_account.py` are:

    # bank_account.py
    
    def close():
        print("We are sorry to see you go.")
    
    def open():
        print("Thank you for opening an account.")

And now we want to call the `open()` function from within `main.py`. The simplest way to do this is to import the `bank_account` module:

    import bank_account
    
    bank_account.open()

We refer to `bank_account` as the **namespace** of `open()` and `close()`.

> **Warning**: Do not confuse **namespace** with **implicit namespace package**. They're two different things.

At a certain point, however, namespaces can become a pain, especially with nested packages. `bank_app.retail_banking.customer.bank_account.open()` is just ugly. Thankfully, we do have a way around having to use the namespace _every time_ we call the function.

    from bank_account import open
    
    open()

It is important to note, that neither `close()` nor `bank_account.close()` will work in this last scenario. This is because we did not import the function outright. To gain access to `close()` we have to import it as well.

    from bank_account import open, close
    
    open()
    close()

This approach is extremely useful with longer namespace names.

    from bank_app.retail_banking.customer.bank_account import open
    
    open()

Or alternatively,

    from bank_app.retail_banking.customer import bank_account
    
    bank_account.open()

The great thing about the `import` system is that it is extremely flexible.

I should mention, this type of package nesting is not favoured by Python developers. When given the choice, flat is better than nested.

### Explicit is better than implicit

We do have the ability to import everything from a module, however, this highly frowned upon. And here is why.

    from bank_account import *
    from gzip import *
    
    open()

As `gzip` has an `open()` function, and since it was the last module imported, it is that function that gets called. Our `bank_account.open()` has been **shadowed**, which means that we cannot call it at all.

Hence, explicit is better than implicit. You should never have to guess where a function or variable is coming from.

### Taking a step back ..

If I want to use the `TodoStatus` class defined in `todo/common/status_enums.py` in the example project structure above, how would  you import this? Well, since I organised my modules as subpackages, I would use an **absolute import**. 

    from todo.common.status_enums import TodoStatus

An **absolute import** starts at the top-level package and then walks down into the `common` package, where it finds `status_enums.py`.

What happens if we try to import `common` without including the top-level package name? Well it won't work. Simply put, in our example, the `data` package has no knowledge of its siblings, but it does know about its parent. So instead we can use a **relative import**.

    from ..common.status_enums import TodoStatus

The `..` means "this package's direct parent package", which in this case, is `todo`. So, the import steps back one level, walks down into `common`, and finds `status_enums.py`.

There is a lot of debate about whether to use absolute or relative imports. Personally, I prefer to use absolute imports whenever possible, because it makes the code a lot more readable. 

One clear advantage of relative imports is that they are quite succinct. Depending on the current location, they can turn the extremely long import statements to something much simpler. Unfortunately, relative imports can be messy, particularly for shared projects where directory structure is likely to change.

You can make up your own mind, however. The only important part is that the result is obvious - there should be no mystery where anything comes from.

## Import style

[PEP 8](http://pep8.org/#imports), the official style guide for Python, has a few pointers when it comes to writing import statements. Let us consider the below example.

    import json, logging
    from dataclasses import asdict
    import redis
    from allocation import config, views
    from allocation.domain import events

1. Imports should always be written at the top of the file, after any module comments and docstrings.
2. Imports should usually be on separate lines.
3. Imports should be divided according to what is being imported. There are generally three groups:
   * Python’s built-in Standard Library modules
   * Related third party imports. These are modules that you take a dependency on but are not directly part of your application.
   * Local application imports. The modules that belong to your application.
4. Each group of imports should be separated by a blank space.
5. Absolute imports are recommended, as they are usually more readable and tend to be better behaved

It’s also a good idea to order your imports alphabetically within each import group. This makes finding particular imports much easier, especially when there are many imports in a file.

    """This is the summary line
    
    This is the further elaboration of the docstring. Within this section,
    you can elaborate further on details as appropriate for the situation.
    """
    
    import json
    import logging
    
    import redis
    
    from allocation import config, views
    from allocation.domain import events
    from data_classes import asdict

## __main__.py

## In the end

Thanks for reading!
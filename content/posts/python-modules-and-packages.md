---
aliases: "/python-modules-and-packages"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Paul Teysen on Unsplash"
  image: "/posts/images/python-modules-and-packages.jpg"
  relative: false
date: 2021-02-03T13:24:18.000+00:00
description: "Understanding the basics"
disableShare: false
draft: true
hideMeta: false
showToc: true
tags: [  "beginners", "python", "best-practices" ]
title: "Python Modules and Packages"
tocOpen: true
---

Thanks to the way imports and modules are handled in Python, it is relatively easy to [structure a Python project](https://www.goodcodebadcode.dev/posts/structure-your-Python-projects). What we mean here is that you do not have many constraints and that the module importing model is easy to grasp. Therefore, you are left with the pure architectural task of crafting the different parts of your project and their interactions.

Easy structuring of a project means it is also easy to do it poorly. Some signs of a poorly structured project include:

* Multiple and messy circular dependencies
* Hidden coupling
* Heavy usage of global state or context
* Spaghetti code
* Ravioli code where hundreds of similar little pieces of logic, often classes or objects, without proper structure.

## Modules

Any Python (`.py`) file is a module, and a collection of modules in a single directory with an `__init__.py` file is a package. This file serves many purposes, but for simplicity sakes, it tells the Python interpreter that this directory is a package directory.

This means that if you have two files in the same folder you can load the definitions and statements from one module for use in the other module.

In short, **modules are named by filenames**, and **packages are named by their directory name**.

How does this translate in a real world example - let’s take a look at the Flask tutorial project [Flaskr](https://flask.palletsprojects.com/en/1.1.x/tutorial/):

```no-highlight
flaskr-tutorial/
├── flaskr/
│   ├── ___init__.py
│   ├── db.py
│   ├── schema.sql
│   ├── auth.py
│   ├── blog.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── auth/
│   │   │   ├── login.html
│   │   │   └── register.html
│   │   └── blog/
│   │       ├── create.html
│   │       ├── index.html
│   │       └── update.html
│   └── static/
│       └── style.css
├── tests/
│   ├── conftest.py
│   ├── data.sql
│   ├── test_factory.py
│   ├── test_db.py
│   ├── test_auth.py
│   └── test_blog.py
├── venv/
├── .gitignore
├── setup.py
└── MANIFEST.in
```

Here you can see that a Flask application, like most Python applications, is built around Python modules and packages.

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

When Python finds the module, it binds it to a name in the local scope. This means that the import is now defined and can be used in the current module without throwing a `NameError`.

If the name is never found, you will get a `ModuleNotFoundError`. 

> You can find out more about imports in the [Python documentation](https://docs.python.org/3/reference/import.html).

### The standard library

[Python's Standard Library](https://docs.python.org/3/library/) is very extensive. The library contains built-in modules, written in C, that provide access to system functionality such as file I/O that would otherwise be inaccessible to Python programmers. 

It also includes other modules written in Python that provide standardised solutions for many problems that occur in everyday programming.

## Syntax of Import Statements

There are a number of ways of importing, but there are few written and unwritten rules that you should follow.

For practical purposes, let us assume we have the following two files: `customer.py` and `bank_account.py` located in the same directory.

```no-highlight
banking-app/
├── bank_account.py
└── customer.py
```

The contents of `bank_account.py` are:

```python
# bank_account.py

def close():
    print("We are sorry to see you go.")

def open():
    print("Thank you for opening an account.")
```

And now we want to call the `open()` function from within `customer.py`. The simplest way to do this is to import the `bank_account` module:

```python
import bank_account

bank_account.open()
```

We refer to `bank_account` as the **namespace** of `open()` and `close()`.

> **Warning**: Do not confuse **namespace** with **implicit namespace package**. They're two different things.

### Flat is better than nested

At a certain point, however, namespaces can become a pain, especially with nested packages. `bank_app.retail_banking.customer.bank_account.open()` is just ugly. Thankfully, we do have a way around having to use the namespace every time we call the function.

```python
from bank_account import open

open()
```

It is important to note, that neither `close()` nor `bank_account.close()` will work in this last scenario. This is because we did not import the function outright. To gain access to `close()` we have to import it as well.

```python
from bank_account import open, close

open()
close()
```

This approach is extremely useful with longer namespace names.

```python
from bank_app.retail_banking.customer.bank_account import open

open()
```

Or alternatively,

```python
from bank_app.retail_banking.customer import bank_account

bank_account.open()
```

The great thing about the `import` system is that it is extremely flexible.

I should mention, this type of package nesting is not favoured by Python developers. When given the choice, flat is better than nested.

### Explicit is better than implicit

We do have the ability to import everything from a module, however, this highly frowned upon. And here is why.

```python
from bank_account import *
from gzip import *

open()
```

As `gzip` has an `open()` function, and since it was the last module imported, it is that function that gets called. Our `bank_account.open()` has been **shadowed**, which means that we cannot call it at all.

Hence, explicit is better than implicit. You should never have to guess where a function or variable is coming from.

### Absolute vs Relative imports

Let's say you wanted to import the `TodoStatus` class defined in a module located in the package `todo/common/status_enums.py`, how would you import this? Well, since the modules are arranged as subpackages, we would use an **absolute import**.

```python
from todo.common.status_enums import TodoStatus
```

An **absolute import** starts at the top-level package and then walks down into the `common` package, where it finds `status_enums.py`.

What happens if we try to import `common` without including the top-level package name? Well it won't work. Simply put, in our example, the `data` package has no knowledge of its siblings, but it does know about its parent. So instead we can use a **relative import**.

```python
from ..common.status_enums import TodoStatus
```

The `..` means "this package's direct parent package", which in this case, is `todo`. So, the import steps back one level, walks down into `common`, and finds `status_enums.py`.

There is a lot of debate about whether to use absolute or relative imports. Personally, I prefer to use absolute imports whenever possible, because it makes the code a lot more readable.

One clear advantage of relative imports is that they are quite succinct. Depending on the current location, they can turn the extremely long import statements to something much simpler. Unfortunately, relative imports can be messy, particularly for shared projects where directory structure is likely to change.

You can make up your own mind, however. The only important part is that the result is obvious - there should be no mystery where anything comes from.

## PEP 8 -- The Style Guide for Python Code

PEP stands for **Python Enhancement Proposal**, and there are several of them. A PEP is a document that describes new features proposed for Python and documents aspects of Python, like design and style, for the community.

[PEP 8](https://www.python.org/dev/peps/pep-0008/), sometimes spelled PEP8 or PEP-8, is a document that provides guidelines and best practices on how to write Python code. It was written in 2001 by Guido van Rossum, Barry Warsaw, and Nick Coghlan. The primary focus of PEP 8 is to improve the readability and consistency of Python code.

> Guido Van Rossum says, "A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important."

Now, not all PEPs are actually adopted, of course - that's why they're called "Proposals" - but some are. You can browse the master PEP index on the official Python website. This index is formally referred to as [PEP 0](https://www.python.org/dev/peps/).

### Naming Packages and Modules

Right now, we're mainly concerned with the section entitled [Package and Module Names](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).

It says that, **module names should be short and all lowercase, with underscores** if that improves readability. Similarly, **package names should be all lowercase, without underscores** if at all avoidable.

### Import Style

[PEP 8](https://www.python.org/dev/peps/pep-0008/#imports) also has a few pointers when it comes to writing import statements. Let us consider the below example.

```python
import json, logging
from dataclasses import asdict
import redis
from allocation import config, views
from allocation.domain import events
```

It suggests that imports should usually **be on separate lines** and should **always be written at the top of the file**, after any module comments and docstrings.

It’s also a good idea to **group your imports** according to what is being imported (this includes Python’s built-in Standard Library modules, related third party imports, and local application imports) and to **order them alphabetically** within each import group. This makes finding particular imports much easier, especially when there are many imports in a file. **Each group should be separated by a blank space**.

And to improve readability and behaviour, **absolute imports are recommended**.

```python
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
```

## if name == "\_\_main\_\_":

You see this a lot in Python and it confuses most people new to the language. Whilst Python does not have much [boilerplate](https://en.wikipedia.org/wiki/Boilerplate_code), this is one of those rare bits.

### Special variables

When the Python interpreter runs a file, it first defines a few special variables. In this case, we care about the `__name__` variable.

If you are running your module as the main program, the interpreter will assign the hard-coded string `"__main__"` to the `__name__` variable.

```python
# It's as if the interpreter inserts this at the top
# of your module when run as the main program.
__name__ = "__main__" 
```

On the other hand, suppose some other module is the main program and it imports your module. This means there's a statement like this in the main program, or in some other module the main program imports.

So, using our earlier import example.

```python
import bank_account
```

The interpreter will search for your `bank_account.py` file, and prior to executing that module, it will assign the name `bank_account` from the import statement to the `__name__` variable.

```python
# It's as if the interpreter inserts this at the top
# of your module when it's imported from another module.
__name__ = "bank_account"
```

After the special variables are set up, the interpreter executes all the code in the module, one statement at a time.

So, `if __name__ == "__main__":` is actually checking if the module is being executed as the _main_ module. If it is, it runs the code under the conditional.

### But why?

You might naturally wonder why anybody would want to do this. Well, sometimes you want to write a `.py` file that can be both used by other programs and/or modules as a module, and can also be run as the main program itself, so for example:

* Your module is a library, but you want to have a script mode where it runs some unit tests or a demo.
* Your module is only used as a main program, but it has some unit tests, and the testing framework works by importing `.py` files like your script and running special test functions. You don't want it to try running the script just because it's importing the module.
* Your module is mostly used as a main program, but it also provides a programmer-friendly API for advanced users.

Beyond those examples, it's elegant that running a script in Python is just setting up a few magic variables and importing the script. "Running" the script is a side effect of importing the script's module.

## Conclusion

Of course, there are a lot more advanced concepts and tricks we can employ in structuring a Python project, but we won't be discussing that here.

* Every Python code file (`.py`) is a **module**.
* Organise your modules into **packages**. Each package must contain a special `__init__.py` file.
* Your project should generally consist of one top-level package, usually containing sub-packages. That top-level package usually shares the name of your project, and exists as a directory in the root of your project's repository.
* **NEVER EVER EVER** use `*` in an import statement. Before you entertain a possible exception, [the Zen of Python](https://www.python.org/dev/peps/pep-0020/) points out "Special cases aren't special enough to break the rules."
* Use absolute or relative imports to refer to other modules in your project.
* Executable projects should have a `__main__.py` in the top-level package. Then, you can directly execute that package with `python -m myproject`.

### Useful resources

* [Python Reference: the import system](https://docs.python.org/3/reference/import.html)
* [Python Tutorials: Modules](https://docs.python.org/3/tutorial/modules.html)
* [PEP 8: Style Guide for Python](https://www.python.org/dev/peps/pep-0008/)
* [PEP 20: The Zen of Python](https://www.python.org/dev/peps/pep-0020/)
* [PEP 240: Implicit Namespace Packages](https://www.python.org/dev/peps/pep-0420/)
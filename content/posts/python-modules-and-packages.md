---
aliases: "/python-modules-and-packages"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Paul Teysen on Unsplash"
  image: "/posts/images/python-modules-and-packages.jpg"
  relative: false
date: 2021-02-03T13:24:18.000+00:00
description: "Thanks to the way imports and modules are handled in Python, it is relatively easy to structure a Python project."
disableShare: false
draft: true
hideMeta: false
showToc: false
tags: [  "beginners", "python", "best-practices" ]
title: "Python Modules and Packages"
tocOpen: false
---

Thanks to the way imports and modules are handled in Python, it is relatively easy to structure a Python project. Easy, here, means that you do not have many constraints and that the module importing model is easy to grasp. Therefore, you are left with the pure architectural task of crafting the different parts of your project and their interactions.

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

Let’s take a look at a simple todo project:

{{< highlight no-highlight >}}

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

{{< /highlight >}}

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

{{< highlight no-highlight >}}

bank_app/
├── bank_account.py
└── main.py

{{< /highlight >}}

The contents of `bank_account.py` are:

{{< highlight python >}}
# bank_account.py

def close():
    print("We are sorry to see you go.")

def open():
    print("Thank you for opening an account.")

{{< /highlight >}}

And now we want to call the `open()` function from within `main.py`. The simplest way to do this is to import the `bank_account` module:

```python
import bank_account

bank_account.open()
```

We refer to `bank_account` as the **namespace** of `open()` and `close()`.

> **Warning**: Do not confuse **namespace** with **implicit namespace package**. They're two different things.

At a certain point, however, namespaces can become a pain, especially with nested packages. `bank_app.retail_banking.customer.bank_account.open()` is just ugly. Thankfully, we do have a way around having to use the namespace _every time_ we call the function.

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

### Taking a step back ..

If I want to use the `TodoStatus` class defined in `todo/common/status_enums.py` in the example project structure above, how would  you import this? Well, since I organised my modules as subpackages, I would use an **absolute import**.

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

## Import style

[PEP 8](http://pep8.org/#imports), the official style guide for Python, has a few pointers when it comes to writing import statements. Let us consider the below example.

```python
import json, logging
from dataclasses import asdict
import redis
from allocation import config, views
from allocation.domain import events
```

1. Imports should always be written at the top of the file, after any module comments and docstrings.
2. Imports should usually be on separate lines.
3. Imports should be divided according to what is being imported. There are generally three groups:
   * Python’s built-in Standard Library modules
   * Related third party imports. These are modules that you take a dependency on but are not directly part of your application.
   * Local application imports. The modules that belong to your application.
4. Each group of imports should be separated by a blank space.
5. Absolute imports are recommended, as they are usually more readable and tend to be better behaved

It’s also a good idea to order your imports alphabetically within each import group. This makes finding particular imports much easier, especially when there are many imports in a file.

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

## if name == "__main__":

You see this a lot in Python and it confuses most folks. Whilst Python does not have much **boilerplate** - code that must be used pretty universally with little to no modification - but this is one of those rare bits.

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

## In the end

Let's review.

* Every project should use a VCS, such as Git. There are plenty of options to choose from.
* Every Python code file (`.py`) is a **module**.
* Organise your modules into **packages**. Each package must contain a special `__init__.py` file.
* Your project should generally consist of one top-level package, usually containing sub-packages. That top-level package usually shares the name of your project, and exists as a directory in the root of your project's repository.
* **NEVER EVER EVER** use `*` in an import statement. Before you entertain a possible exception, the Zen of Python points out "Special cases aren't special enough to break the rules."
* Use absolute or relative imports to refer to other modules in your project.
* Executable projects should have a `__main__.py` in the top-level package. Then, you can directly execute that package with `python -m myproject`.

Of course, there are a lot more advanced concepts and tricks we can employ in structuring a Python project, but we won't be discussing that here. I highly recommend reading the docs:

* [Python Reference: the import system](https://docs.python.org/3/reference/import.html)
* [Python Tutorials: Modules](https://docs.python.org/3/tutorial/modules.html)
* [PEP 8: Style Guide for Python](https://www.python.org/dev/peps/pep-0008/)
* [PEP 20: The Zen of Python](https://www.python.org/dev/peps/pep-0020/)
* [PEP 240: Implicit Namespace Packages](https://www.python.org/dev/peps/pep-0420/)
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
draft: false
hideMeta: false
showToc: true
tags: [  "beginners", "python", "best-practices" ]
title: "Python Modules and Packages"
tocOpen: true
---

If you are new to the Python language, you have probably coded using the [Python Interpreter](https://docs.python.org/3/tutorial/interpreter.html). Once you progress to building longer programs you will start writing scripts. As your program grows in size you will want to split it into several files for easier maintenance and reusability of the code. The solution to this is modules. 

Thanks to the way [imports](https://docs.python.org/3/reference/import.html) and modules are handled in Python, it is relatively easy to [structure a Python project](https://www.goodcodebadcode.dev/posts/structure-your-Python-projects). 

What we mean here is that you do not have many constraints and that the module importing model is easy to grasp. Therefore, you are left with the pure architectural task of crafting the different parts of your project and their interactions.

## Modules

A module contains a set of function definitions, variables and statements that you want to include in your application.

Any Python (`.py`) file is a module, and a collection of modules in a single directory with an `__init__.py` file is a package. This file serves many purposes, but for simplicity, it tells the Python Interpreter that this directory is a package directory.

In short, **modules are named by filenames**, and **packages are named by their directory name**.

This means that if you have two files in the same folder you can load the contents from one module for use in the other module.

How does this translate into a real world implementation - let’s take a look at an example Accounts API based on the [Open Banking Project](https://www.openbankproject.com/):

```no-highlight
obp-api
├── docs/
├── services/
│   ├── accounts/
│   │   ├── __init__.py
│   │   ├── bank_account.py
│   │   └── routes.py
│   ├── app.py
│   ├── auth.py
│   ├── common/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── check.py
│   │   ├── route_constants.py
│   │   ├── serializers.py
│   │   ├── settings.py
│   │   └── status_enums.py
│   ├── data/
│   │   ├── __init__.py
│   │   ├── connection_manager.py
│   │   ├── database.py
│   │   ├── db_config.py
│   │   ├── models.py
│   │   └── queries.py
│   ├── __init__.py
│   ├── __main__.py
│   ├── static/
│   └── tests/
│       ├── __init__.py
│       ├── mock_db.py
│       ├── mock_db_config.py
│       ├── test_auth.py
│       ├── test_check.py
│       ├── test_connection_manager.py
│       ├── test_bank_account.py│     
│       ├── test_routes.py
│       ├── test_serializers.py
│       └── test_transfers.py
├── .coveragerc
├── .gitignore
├── docker-compose.deploy.yml
├── docker-compose.yml
├── LICENSE
├── makefile
├── pylintrc
├── README.rst
├── requirements.txt
└── setup.py
```

Here you can see that the application structure, like most Python applications, is built around Python modules and packages.

## Importing modules

What happens when you have code in one module that needs to access code in another module or package? You import it!

When you import a module, we are actually running it. This means that any recursive or inner `import` statements within the module are also run.

```no-highlight
import re
```

So when we import `re.py` into a module, we are in fact importing `re.py` and its own several import statements. That doesn't mean that those imports are available to us in our module, but it does mean those files have to exist. If, for some unlikely reason, `enum.py` was deleted on your environment, and you ran `import re`, it would fail with an error.

> The [re](https://docs.python.org/3/library/re.html) module provides support for regular expressions and matching operations. See the [source on GitHub](https://github.com/python/cpython/blob/master/Lib/re.py).

I have often heard newer Python developers ask why the inner module, `enum.py` in our example is being imported, since they didn't ask for it directly in their code. The answer is simple: we imported `re`, and that imports `enum`.

### Import paths

You have probably noticed that we did not provide a file path for the import, yet somehow Python knew where to find these.

The first thing Python will do is look up the name of the module in `sys.modules`. This is a cache of all modules that have been previously imported.

If the name is not found in the module cache, Python will proceed to search through a list of built-in modules. These are modules that come pre-installed with Python and can be found in the [Python Standard Library](https://docs.python.org/3/library/). If the name still isn’t found in the built-in modules, Python then searches for it in a list of directories defined by `sys.path`. This list usually includes the current directory, which is searched first.

You are allowed to modify `sys.path` during run-time. Just be sure to modify it before you call import. It will search the directories in order stopping at the first place it finds the specified modules.

When Python finds the module, it binds it to a name in the local scope. This means that the import is now defined and can be used in the current module without throwing a `NameError`.

If the name is never found, you will get a `ModuleNotFoundError`. 

### Symbol Tables

Once the module is found, the calling module still does not have direct access to the imported module's objects directly.

Each module as its own __symbol table__ which serves as the __global symbol table__ for all objects defined in the module. 

The import statement places the imported module into the caller’s symbol table. The objects that are defined in the imported module remain in the imported module’s private symbol table.

From the caller, the objects in the module are only accessible when prefixed with module's name via dot notation. Let's look at an example by importing the Standard Library's [math module](https://docs.python.org/3/library/math.html).

```no-highlight
>>> import math
>>> math
<module 'math' from '/lib/python3.7/lib-dynload/math.cpython-37m-darwin.so'>
```

After the import statement, `math` is placed into the local symbol table, and therefore has meaning in the caller’s local context. However, data items such as `pi` remain in the imported module’s private symbol table and are not meaningful in the local context.

```no-highlight
>>> pi
Traceback (most recent call last):
  File "<pyshell#7>", line 1, in <module>
    pi
NameError: name 'pi' is not defined
```

To be accessed in the local context, names of objects defined in the imported module must be prefixed by the module name.

```no-highlight
>>> math.pi
3.141592653589793
```

### Security risks

You should be aware that Python’s import system presents some significant security risks. This is largely due to its flexibility. For example, the module cache is writable, and it is possible to override core Python functionality using the import system. Importing from third-party packages can also expose your application to security threats.

The article [10 common security gotchas in Python and how to avoid them](https://hackernoon.com/10-common-security-gotchas-in-python-and-how-to-avoid-them-e19fbe265e03) by Anthony Shaw discusses these security concerns and how to mitigate them. Point 5 talks about Python’s import system.

### The Standard Library

You have probably noticed by now that we have referenced the [Python Standard Library](https://docs.python.org/3/library/) numerous times. So what is the Standard Library? 

The Standard Library comes bundled with the core Python distribution and contains built-in modules, written in C, that provide access to system functionality such as file I/O that would otherwise be inaccessible to Python programmers. It also includes other modules written in Python that provide standardised solutions for many problems that occur in everyday programming.

There are more than 200 core modules at the heart of the Standard Library. In a future post, we will take a look at the most important modules.

## Syntax of Import Statements

There are a number of ways of importing modules. Let us start by looking at basic import syntax.

Assuming we have the following code saved in the module `bank_account.py`:

```no-highlight
def close():
    print('We are sorry to see you go.')


def open():
    print('Thank you for opening an account.')
```

If we want to access the `open()` function or the any of the other function definitions, variables or statements, we have to first import the module.

For practical purposes, we will run the rest of the code in this section in the Python interactive shell, from the same directory as `bank_account.py`.

```no-highlight
>>> import bank_account
>>> bank_account.open()
Thank you for opening an account.
>>> 
```

### Importing names as an alias

It is also possible to import entire modules or individual objects but enter them into the local symbol table with alternate names by using the `as` keyword. 

This is useful if you have already used the same name for something else in your program, another module you have imported also uses that name, or you may want to abbreviate a longer name that you are using a lot.

```no-highlight
>>> import bank_account as ba
>>> ba.open()
Thank you for opening an account.
```

For some modules, it is commonplace to use aliases. The `matplotlib.pyplot` module’s [official documentation](https://matplotlib.org/3.3.4/api/_as_gen/matplotlib.pyplot.html) recommends for use of `plt` as an alias.

### Importing specific names

Another alternative form of the import statement allows individual objects from the module to be imported directly into the caller’s symbol table.

```no-highlight
>>> from bank_account import open
>>> open()
Thank you for opening an account.
```

Here, we imported only the `open()` function from the `bank_account` module. As you can see, the `bank_account` prefix can now be omitted.

It is important to note, that neither `close()` nor `bank_account.close()` will work in this last scenario. This is because we did not import the function outright. 

```no-highlight
>>> from bank_account import open
>>> close()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'close' is not defined
```

To gain access to `close()` we have to import it as well.

```no-highlight
>>> from bank_account import open, close
>>> close()
We are sorry to see you go.
```

This approach is extremely useful with longer namespace names.

```no-highlight
>>> from obp.services.accounts import bank_account
>>> bank_account.open()
Thank you for opening an account.
```

### Import all names

It is even possible to indiscriminately import everything from a module at once.

```no-highlight
>>> from bank_account import *
>>> open()
Thank you for opening an account.
>>> close()
We are sorry to see you go.
```

This will place the names of all objects from `bank_account` into the local symbol table, with the exception of any that begin with the underscore (`_`) character. It must be said that this approach is highly frowned upon. And here is why.

```no-highlight
>>> from bank_account import *
>>> from gzip import *
>>> open()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: open() missing 1 required positional argument: 'filename'
```

[Gzip](https://docs.python.org/3/library/gzip.html#examples-of-usage) also has an `open()` function, and since it was the last module imported, it is that function that gets called. Our `bank_account.open()` function has been **shadowed**, which means that we cannot call it at all.

Hence, **explicit is better than implicit** (Peters, 2004). You should never have to guess where a function or variable is coming from.

## Namespaces

In all of these examples, we imported `bank_account`. This is referred to as the **namespace** of `open()` and `close()`. 

> Do not confuse **namespace** with [implicit namespace packages](https://packaging.python.org/guides/packaging-namespace-packages/). They are two different things.

At a certain point, however, namespaces can become a pain, especially with nested packages.  `obp.services.accounts.bank_account.open()` is just not very practical and impacts the readability of our code. Thankfully, the import system is extremely flexible.

### Absolute imports

Let's say you wanted to import the `AccountStatus` class defined in the module located in the package `services/common/status_enums.py`, how would you import this? 

Since `services` is defined as a package, with the other modules organised into sub-packages, we would use an **absolute import**.

An absolute import starts at the top-level package and then walks down into the `common` package, where it finds `status_enums.py`. Similar to a filepath, but instead of slashes, we use a dot.

```no-highlight
>>> from services.common.status_enums import AccountStatus
```

### Relative imports

Now what happens if we try to import `common` without including the top-level package name? Well it won't work. Simply put, in our example, the `accounts` package has no knowledge of its siblings, but it does know about its parent. So instead we can use a **relative import**.

```no-highlight
>>> from ..common.status_enums import AccountStatus
```

The `..` means "this package's direct parent package", which in this case, is `services`. So, the import steps back one level, walks down into `common`, and finds `status_enums.py`. 

### Syntax of Relative imports

The syntax of a relative import depends on the current location as well as the location of the module, package, or object to be imported. 

```no-highlight
>>> from .db_config import user
>>> from ..common.status_enums import AccountStatus
>>> from . import AccountModel
```

A single dot means that the module or package referenced is in the same directory as the current location. Two dots mean that it is in the parent directory of the current location—that is, the directory above. Three dots mean that it is in the grandparent directory, and so on. This will probably be familiar to you if you use a Unix-like operating system!

### Pros and Cons

One clear advantage of relative imports is that they are quite succinct. Depending on the current location, they can turn the extremely long import statements to something much simpler. Unfortunately, relative imports can be messy, particularly for shared projects where directory structure is likely to change.

There is a lot of debate about whether to use absolute or relative imports. Personally, I prefer to use absolute imports whenever possible, because they are quite clear and straightforward. It is easy to tell exactly where the imported resource is, just by looking at the statement. 

You can make up your own mind, however. The only important part is that the result is obvious - **there should be no mystery where anything comes from**  (Rossum, 2001).

## PEP 8 -- The Style Guide for Python Code

PEP stands for **Python Enhancement Proposal**, and there are several of them. A PEP is a document that describes new features proposed for Python and documents aspects of Python, like design and style, for the community.

[PEP 8](https://www.python.org/dev/peps/pep-0008/), sometimes spelled PEP8 or PEP-8, is a document that provides guidelines and best practices on how to write Python code. It was written in 2001 by Guido van Rossum, Barry Warsaw, and Nick Coghlan. The primary focus of PEP 8 is to improve the readability and consistency of Python code.

> Guido Van Rossum says, "A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important."

Now, not all PEPs are actually adopted, of course - that's why they're called "Proposals" - but some are. You can browse the master PEP index on the official Python website. This index is formally referred to as [PEP 0](https://www.python.org/dev/peps/).

### Naming Packages and Modules

Right now, we're mainly concerned with the section entitled [Package and Module Names](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).

It says that, **module names (files) should be short and all lowercase, with underscores** if that improves readability. Similarly, **package names (directories) should be all lowercase, without underscores** if at all avoidable.

In other words, do this `services/data/route_constants.py` and NOT `services/Data/RouteConstants.py`.

### Import Style

[PEP 8](https://www.python.org/dev/peps/pep-0008/#imports) also has a few pointers when it comes to writing import statements. Let us consider the below example:

It suggests that imports should usually **be on separate lines** and should **always be written at the top of the file**, after any module comments and docstrings.

It’s also a good idea to **group your imports** according to what is being imported (this includes Python’s built-in Standard Library modules, related third party imports, and local application imports) and to **order them alphabetically** within each import group. This makes finding particular imports much easier, especially when there are many imports in a file. **Each group should be separated by a blank space**.

Here’s an example of how to style import statements:

```no-highlight
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

## Python Packages

Packages allow for a **hierarchical structuring** of the module namespace using dot notation. In the same way that modules help avoid collisions between global variable names, packages help avoid collisions between module names.

As we mentioned already, **a collection of modules in a single directory with an `__init__.py` file is a package**.

### Package Initialization

When the `__init__.py` file is present in a directory, it is invoked when the package or a module in the package is imported. This can be used for execution of package initialization code, such as initialization of package-level data.

For example, consider the following `__init__.py` file.

```no-highlight
print(f"Invoking __init__.py for {__name__}")
A = ['hello', 'world']
```

Let’s add this to an `example_pkg` directory and create two modules, `foo.py` and `bar.py`. Your directory should look like this.

```no-highlight
example_pkg
├── __init__.py
├── foo.py
└── bar.py
```

The contents of the two modules are.

```no-highlight
def foo():
    print('foo()')

class Foo:
    pass
```

```no-highlight
def bar():
    print('bar()')

class Bar:
    pass
```

Now when the package is imported, the global list `A` is initialized.

```no-highlight
>>> import example_pkg
Invoking __init__.py for example_pkg
>>> example_pkg.A
['hello', 'world']
```

A **module** in the package can then access the global list `A` by importing it in turn. Let's slightly modify the contents of `foo.py`.

```no-highlight
def foo():
    from example_pkg import A
    print('foo() / A = ', A)

class Foo:
    pass
```

```no-highlight
>>> from example_pkg import foo
Invoking __init__.py for example_pkg
>>> foo.foo()
foo() / A = ['hello', 'world']
```

`__init__.py` can also be used to effect automatic importing of modules from a package. For example, earlier you saw that the statement `import example_pkg` only places the name `example_pkg` in the caller’s local symbol table and doesn’t import any modules. But if `__init__.py` in the `example_pkg` directory contains the following.

```no-highlight
print(f"Invoking __init__.py for {__name__}")
import example_pkg.foo, example_pkg.bar
```

Then when you execute `import example_pkg`, modules `foo` and `bar` are automatically imported.

```no-highlight
>>> import example_pkg
Invoking __init__.py for example_pkg
>>> example_pkg.foo.foo()
foo()
>>> example_pkg.bar.bar()
bar()
```

> Starting with Python 3.3, [Implicit Namespace Packages](https://www.python.org/dev/peps/pep-0420/) were introduced. These allow for the creation of a package without any `__init__.py` file. Of course, it can still be present if package initialization is needed. But it is no longer required.

## Special variables

Python comes with a number of special variables and methods whose name is preceded and followed by `__`. In fact, because of this, it's recommended that you don't write your own variables using this pattern as to not confuse the Python Interpreter.

In the case of imports, we care about the `__name__` and `__main__` variables.

If you are running your module as the main program, the interpreter will assign the hard-coded string `'__main__'` to the `__name__` variable.

```no-highlight
# It's as if the interpreter inserts this at the top
# of your module when run as the main program.
__name__ = '__main__' 
```

On the other hand, suppose some other module is the main program and it imports your module. This means there's a statement like this in the main program, or in some other module the main program imports.

So, using our earlier import examples.

```no-highlight
import bank_account
```

The interpreter will search for the `bank_account.py` file, and prior to executing that module, it will assign the name `bank_account` from the import statement to the `__name__` variable.

```no-highlight
# It's as if the interpreter inserts this at the top
# of your module when it's imported from another module.
__name__ = 'bank_account'
```

After the special variables are set up, the interpreter executes all the code in the module, one statement at a time.

So, `if __name__ == '__main__':` is actually checking if the module is being executed as the _main_ module. If it is, it runs the code under the conditional.

You might naturally wonder why anybody would want to do this. Well, sometimes you want to write a `.py` file that can be both used by other programs and/or modules as a module, and can also be run as the main program itself.

Take this example, let us assume we have a simple module called `my_module.py` which has the following contents:

```no-highlight
print('This will run when the file is imported.')

def my_function():
    print('Executing function. This will only run when the function is called.')

if __name__ == '__main__':
    print('This will get executed only if the module is invoked directly.')
    print('It will not run when this module is imported')
    my_function()
```

Now, let us invoke the `my_module` as the main module with the `-m` flag:

```no-highlight
$ python -m my_module
This will run when the file is imported.
This will get executed only if the module is invoked directly.
It will not run when this module is imported
Executing function. This will only run when the function is called.
```

You can see that since `my_module` is the main module in this case, the condition `if __name__ == '__main__':` is evaluated as `true` and the code is executed. 

Now, if we import `my_module` so that it is not the main module:

```no-highlight
$ python -c "import my_module"
This will run when the file is imported.
```

See that only the print statement is executed. 

And finally, if we import `my_module` and call the function defined:

```no-highlight
$ python -c "import my_module; my_module.my_function()"
This will run when the file is imported.
Executing function. This will only run when the function is called.
```

## Conclusion

Of course, there are a lot more advanced concepts and tricks we can employ in structuring a Python project's modules and packages, but we won't be discussing that here. Hopefully, you can now confidently import packages and modules from the Python standard library, third party packages, and your own local packages.

Here are the things to remember:

* Every Python code file (`.py`) is a **module**.
* Organise your modules into **packages**. Each package must contain a special `__init__.py` file.
* Your project should generally consist of one top-level package, usually containing sub-packages. That top-level package usually shares the name of your project, and exists as a directory in the root of your project's repository.
* **NEVER EVER EVER** use `*` in an import statement. Before you entertain a possible exception, [the Zen of Python](https://www.python.org/dev/peps/pep-0020/) points out "Special cases aren't special enough to break the rules."
* Use absolute or relative imports to refer to other modules in your project.

### Useful Resources

* [Python Reference: the import system](https://docs.python.org/3/reference/import.html)
* [Python Tutorials: Modules](https://docs.python.org/3/tutorial/modules.html)
* [PEP 8: Style Guide for Python](https://www.python.org/dev/peps/pep-0008/)
* [PEP 20: The Zen of Python](https://www.python.org/dev/peps/pep-0020/)
* [PEP 240: Implicit Namespace Packages](https://www.python.org/dev/peps/pep-0420/)

### References

1. Sturtz, J., n.d. Python Modules and Packages – An Introduction – Real Python. [online] Realpython.com. Available at: <https://realpython.com/python-modules-packages/> [Accessed 15 January 2021].
2. Peters, T., 2004. PEP 20 -- The Zen of Python. [online] Python.org. Available at: <https://www.python.org/dev/peps/pep-0020/> [Accessed 12 February 2021].
3. Van Rossum, G., Warsaw, B, Coghlan, N., 2001. PEP 8 -- Style Guide for Python Code. [online] Python.org. Available at: <https://www.python.org/dev/peps/pep-0008/> [Accessed 2 February 2021].
4. Finer, J., n.d. How to Write Beautiful Python Code With PEP 8 – Real Python. [online] Realpython.com. Available at: <https://realpython.com/python-pep8/> [Accessed 18 January 2021].
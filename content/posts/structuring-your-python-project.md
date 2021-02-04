---
aliases: "/structure-your-Python-projects"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Lance Anderson on Unsplash"
  image: "/posts/images/structuring-your-python-project.jpg"
  relative: false
date: 2021-01-28T20:46:03.000+00:00
description: "Python, though opinionated on syntax and style, is surprisingly flexible when it comes to structuring your applications."
disableShare: false
draft: true
hideMeta: false
showToc: false
tags: [  "beginners", "python", "best-practices" ]
title: "Structure your Python projects"
tocOpen: false
---

Just as code style, API design, and automation are essential for a healthy productive development cycle. Project structure is a crucial part of your application's architecture.

> Martin Fowler describes architecture as: “The highest-breakdown of a system into its parts.”

In practical terms, architecture means code whose logic and dependencies are clear as well as how the files and folders are organised in the filesystem.

## Sample Project Repository

**tl;dr**: This project structure is regularly used by our internal data science teams. The repository is [available on GitHub](https://github.com/goodcodebadcode/pyproj-structure).

There are various types of Python projects that you will inevitably build overtime, this includes command-line applications, one-off scripts, installable packages, and web applications with popular frameworks like Flask and Django.

{{< highlight no-highlight >}}

/apidocs
/docs
/lib
/sample
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

{{< /highlight >}}

As you can see from this sample above, there are not a lot of complex rules here. If your repository is a massive dump of files or a nested mess of directories, any new developer or contributor will have difficulty getting to grips with your code, regardless of how beautiful your documentation might be. 

Many developers and teams get this simple step wrong, often stumbling through a jumble of common mistakes until they arrive at something that at least works.

## Structure of the Repository

Let’s get into some specifics.

* **/apidocs**: Epydoc-generated API docs.
* **/docs**: All your documentation goes here.
* **/lib**: For any C-Language libraries.
* **/sample**: Your module packages are the core focus of your project. They should not be hidden away. If, however, the module consists of only a single file, place it directly in your the root directory of your project.
* **/scripts** or **/bin**: Any command-line related items.
* **/tests**: For your tests.

The hardest choice is whether or not to use a `/src` tree. Python doesn't have a distinction between `/src`, `/lib`, and `/bin` like Java or C has. Since a top-level `/src` directory is seen by some as meaningless, your top-level directory can be the top-level architecture of your application. 

This top-level directory also contains other related files. Some of these files will be new to you, so let’s take a quick look at what each of them does.

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

## Command-Line projects?

Starting with an empty project can be intimidating often leads to wasted time. Here are some proven project structures that I personally use as a starting point for my Python applications.

### One-off script

The following structure will work for all of those cases where you want to create a simple script. It can be easily modified to reflect whatever installation or other tools you use in your developer workflow. This layout will cover you whether you’re creating a pure Python script (that is, one with no dependencies) or using a tool like pip or Pipenv.

{{< highlight no-highlight >}}

helloworld/
│
├── .gitignore
├── helloworld.py
├── LICENSE
├── README.rst
├── requirements.txt
├── setup.py
└── tests.py

{{< /highlight >}}

This is pretty straightforward: everything is in the same directory. The files shown here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum if you plan on using a basic layout like this.

### Installable package

Let’s imagine that our helloworld.py is still the main script to execute, but we've now moved all of our helper and utility methods to a new file called helpers.py.

We are now going to create a helloworld package but keep all the miscellaneous files, such as your README, .gitignore, and so on, at the top directory.

{{< highlight no-highlight >}}

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
├── .gitignore
├── LICENSE
├── README.rst
├── requirements.txt
└── setup.py

{{< /highlight >}}

The only difference here is that our application code is now all held in the helloworld subdirectory. This directory is named after the package and we’ve added a file called `__init__.py`.

This layout is a slimmed down version of Kenneth Reitz’s [samplemod](https://github.com/navdeep-G/samplemod) application structure.

### Application with internal-packages

In larger applications where one or more internal packages are tied together or provide specific functionality to a larger library. We will extend the again to accommodate for this.

{{< highlight no-highlight >}}

helloworld/
│
├── bin/
│
├── docs/
│   ├── hello.rst
│   └── world.rst
│
├── helloworld/
│   ├── __init__.py
│   ├── runner.py
│   ├── hello/
│   │   ├── __init__.py
│   │   ├── hello.py
│   │   └── helpers.py
│   │
│   └── world/
│       ├── __init__.py
│       ├── helpers.py
│       └── world.py
│
├── data/
│   ├── input.csv
│   └── output.xlsx
│
├── tests/
│   ├── hello
│   │   ├── hello_tests.py
│   │   └── helpers_tests.py
│   │
│   └── world/
│       ├── helpers_tests.py
│       └── world_tests.py
│
├── .gitignore
├── LICENSE
└── README.rst

{{< /highlight >}}

The top-level files remain largely the same as in the previous layout. These three layouts should cover most use cases for command-line applications.

## Web applications

Another major use case of Python is web applications. Django and Flask are arguably the most popular web frameworks for Python and are thankfully are a little more concrete when it comes to project structure.
### Django

 One of the nice things about Django is that it will create a project skeleton for you. When you create a new Django project, it will create a directory in your current working directory with the following structure.

{{< highlight no-highlight >}}

project/
│
├── project/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
└── manage.py

{{< /highlight >}}


### Flask 

Flask is a Python web “microframework.” One of the main selling points is that it is very quick to set up with minimal overhead. 

The Flask documentation has suggested layout for their tutorial project - a blogging web application called Flaskr - and we will examine that here from within the main project directory:

{{< highlight no-highlight >}}

flaskr/
│
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
│   │   │
│   │   └── blog/
│   │       ├── create.html
│   │       ├── index.html
│   │       └── update.html
│   │ 
│   └── static/
│       └── style.css
│
├── tests/
│   ├── conftest.py
│   ├── data.sql
│   ├── test_factory.py
│   ├── test_db.py
│   ├── test_auth.py
│   └── test_blog.py
│
├── venv/
│
├── .gitignore
├── setup.py
└── MANIFEST.in

{{< /highlight >}}

Here you can see that a Flask application, like most Python applications, is built around Python packages.

## Conclusions

Thanks for reading.

https://realpython.com/lessons/structuring-python-application/
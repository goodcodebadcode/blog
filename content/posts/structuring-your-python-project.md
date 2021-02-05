---
aliases: "/structure-your-Python-projects"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Lance Anderson on Unsplash"
  image: "/posts/images/structuring-your-python-project.jpg"
  relative: false
date: 2021-01-28T20:46:03.000+00:00
description: "The ultimate setup for your next Python project"
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

If your repository is a massive dump of files or a nested mess of directories, any contributor will have difficulty getting to grips with your code, regardless of how beautiful your documentation might be. 

Many developers and teams get this simple step wrong, often stumbling through a jumble of common mistakes until they arrive at something that at least works.

## Sample Project Repository

**tl;dr**: This project structure is one I regularly use for work. The repository is available on [GitHub](https://github.com/goodcodebadcode/pyproj-structure).

{{< highlight no-highlight >}}

sample_repo/
├── apidoc/
├── docs/
├── lib/
├── sample/ # the name of the application/module
│   ├── app.py
│   ├── __init__.py
│   ├── __main__.py
│   └── resources/
├── scripts/
├── tests/
│   └── test_app.py
├── .editorconfig
├── .gitattributes
├── .gitignore
├── CHANGELOG.rst
├── CONTRIBUTING.rst
├── LICENSE
├── README.rst
├── pylintrc
├── makefile
├── requirements.txt
├── setup.cfg
└── setup.py

{{< /highlight >}}

As you can see from this sample above, there are not a lot of complex rules here. In fact, it is pretty simple. I then tend to add or remove items as required.

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
* **setup.cfg**: Single config file to reduce the problem of config file soup.
* **setup.py**: Can also be used to define dependencies.

The files and folders included here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum if you plan on using a basic layout like this.

Let us now look at different options for the various types of Python projects that you will inevitably build overtime, this includes machine learning/AI projects, command-line applications, one-off scripts, installable packages, and web applications.

## Command-Line projects?

Starting with an empty project can be intimidating often leading to wasted time. Here are some proven project structures for more simple Python applications.

### One-off script

The following structure will work for all of those cases where you want to create a simple script. It can be easily modified to reflect whatever tools you prefer to use in your developer workflow.

{{< highlight no-highlight >}}

sample_repo/
├── .gitignore
├── LICENSE
├── README.rst
├── requirements.txt
├── sample.py
├── setup.py
└── test_sample.py

{{< /highlight >}}

This is pretty straightforward: everything is in the same directory. The files shown here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum.

### Installable package

Let us imagine that our sample.py is still the main script to execute, but we've now moved all of our helper and utility methods to a new file called helpers.py.

We are now going to create a sample package but keep all the miscellaneous files, such as your README, .gitignore, and so on, at the top directory.

{{< highlight no-highlight >}}

sample_repo/
├── sample/
│   ├── __init__.py
│   ├── sample.py
│   └── helpers.py
├── tests/
│   ├── test_sample.py
│   └── test_helpers.py
├── .gitignore
├── LICENSE
├── README.rst
├── requirements.txt
└── setup.py

{{< /highlight >}}

The only difference here is that our application code is now all held in the sample subdirectory. This directory is named after the package and we’ve added the required `__init__.py` file.

This layout is a slimmed down version of Kenneth Reitz’s [samplemod](https://github.com/navdeep-G/samplemod) application structure.

### Application with internal-packages

In larger applications where one or more internal packages are tied together or provide specific functionality to a larger library. We will extend the again to accommodate for this.

{{< highlight no-highlight >}}

sample_repo/
├── bin/
├── docs/
│   ├── hello.rst
│   └── world.rst
├── sample/
│   ├── __init__.py
│   ├── runner.py
│   ├── hello/
│   │   ├── __init__.py
│   │   ├── hello.py
│   │   └── helpers.py
│   └── world/
│       ├── __init__.py
│       ├── helpers.py
│       └── world.py
├── data/
│   ├── input.csv
│   └── output.xlsx
├── tests/
│   ├── hello
│   │   ├── test_hello.py
│   │   └── test_helpers.py
│   │
│   └── world/
│       ├── test_helpers.py
│       └── test_world.py
├── .gitignore
├── LICENSE
└── README.rst

{{< /highlight >}}

The top-level files remain largely the same as in the previous layout. These three layouts should cover most use cases for command-line applications.

## Web applications

Another major use case of Python is web applications. [Django](https://www.djangoproject.com/) and [Flask](https://flask.palletsprojects.com/en/1.1.x/) are arguably the most popular web frameworks for Python and are thankfully are a little more concrete when it comes to project structure.

### Django

When you [create a new Django project](https://docs.djangoproject.com/en/3.1/intro/tutorial01/#creating-a-project), it will create a directory in your current working directory with the following structure.

{{< highlight no-highlight >}}

mysite/
├── mysite/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

{{< /highlight >}}

After you create a new project, a new directory with the name of your project will be created in your current directory. It will by default include these files:

* **manage.py**: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about manage.py in [django-admin and manage.py](https://docs.djangoproject.com/en/3.1/ref/django-admin/).
* **mysite/**: This directory is the actual Python package for your project.
* **mysite/settings.py**: Settings and configuration for your Django project. Django settings will tell you all about how settings work.
* **mysite/urls.py**: The URL declarations for your Django project.
* **mysite/asgi.py:** An entry-point for ASGI-compatible web servers to serve your project.
* **mysite/wsgi.py**: An entry-point for WSGI-compatible web servers to serve your project.

### Flask 

Flask is a Python web “microframework.” One of the main selling points is that it is very quick to set up with minimal overhead. 

The Flask documentation has suggested layout for their [tutorial project](https://flask.palletsprojects.com/en/1.1.x/tutorial/) - a blogging web application called [Flaskr](https://github.com/pallets/flask/tree/1.1.2/examples/tutorial) - and we will examine that here from within the main project directory.

{{< highlight no-highlight >}}

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

{{< /highlight >}}

Here you can see that a Flask application, like most Python applications, is built around Python packages.

## Conclusions

Now you’ve seen example project structures for a number of different application types. Hopefully, you now have the tools to successfully prevent coder’s block by building out your application structure so that you’re not staring at a blank canvas for each and every project.

Because Python is largely non-opinionated when it comes to application layouts, you can customise these examples to better fit your use case.

Of course, there are a lot more advanced concepts and tricks we can employ in structuring a Python project but hopefully you have come away with the understanding that these rules are neither hard-and-fast. Over time and with practice, you’ll develop the ability to build and customise your own useful application layouts.

Thanks for reading.

### Resources

* [Sample Python Module Repository](https://github.com/navdeep-G/samplemod)
* [Pytest Docs](https://docs.pytest.org/en/latest/contents.html)
* [Python Code Quality Authority](https://github.com/PyCQA)
---
aliases: "/structure-your-Python-projects"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Lance Anderson on Unsplash"
  image: "/posts/images/structuring-your-python-project.jpg"
  relative: false
date: 2021-01-28T20:46:03.000+00:00
description: "A guide for structuring your Python projects"
disableShare: false
draft: true
hideMeta: false
showToc: true
tags: [  "beginners", "python", "best-practices" ]
title: "Structure your Python projects"
tocOpen: true
---

All software systems have an architecture. Even if it comprises of just one structure with one element, there is an architecture. There are software systems that don't have a formal design and others that don't formally document the architecture, but even these systems still have an architecture.

It has been over two decades since [POSA1](https://en.wikipedia.org/wiki/Pattern-Oriented_Software_Architecture) was released and interestingly enough the ideas of design are still very applicable in modern software development.

## Why structure matters

So, just as code style, API design and automation are essential for a healthy productive development cycle. Project structure is a crucial part of your application's architecture.

> Martin Fowler describes architecture as: “The highest-breakdown of a system into its parts.”

Changes to a software system are inevitable. The catalyst for change can come from the market, new requirements, changes to business processes, technology advances, and bug fixes, among other things. A reproducible structure and clean code allows you to manage and understand what it would take to make a particular change.

If your repository is a massive dump of files or a nested mess of directories, responding to change becomes more difficult, regardless of how beautiful your documentation might be. Generally, **if the implementation is hard to explain, it's probably a bad idea**.

Many developers and teams get this simple step wrong, often stumbling through a jumble of mistakes until they arrive at something that at least works.

## A basic project repository

**tl;dr**: This project structure is one I regularly use for work. The repository is available on [GitHub](https://github.com/goodcodebadcode/pyproj-structure).

There is no one way to layout a project, so the best course of action is to select and adopt the practices that meet personal preferences and project requirements.

```no-highlight
sample/
├── apidoc/
├── docs/
├── lib/
├── sample/
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
└── setup.py
```

As you can see, there are not a lot of complex rules here. In fact, it is pretty simple. I then add or remove items as required.

## Structure of the repository

Let us get into some specifics.

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

## Command-Line projects

Starting with an empty project can be intimidating often leading to wasted time. So, let us now look at some different options for the other types of Python projects that you will inevitably build overtime.

### One-off script

The following structure will work for all of those cases where you want to create a simple script. It can be easily modified to reflect whatever tools you prefer to use in your developer workflow.

```no-highlight
sample/
├── .gitignore
├── LICENSE
├── README.rst
├── requirements.txt
├── sample.py
├── setup.py
└── test_sample.py
```

This is pretty straightforward: everything is in the same directory. The files shown here are not necessarily exhaustive, but I recommend keeping the number of files to a minimum.

### Installable package

Let us imagine that our `sample.py` is still the main script to execute, but we have now moved all of our helper and utility methods to a new file called `helpers.py`.

We are now going to create a sample package but keep all the miscellaneous files, such as your `README.rst`, `.gitignore`, and so on, at the top directory.

```no-highlight
sample/
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
```

The only difference here is that our application code is now all held in the sample subdirectory. This directory is named after the package and we’ve added the required `__init__.py` file.

This layout is a slimmed down version of [Kenneth Reitz’s samplemod](https://github.com/navdeep-G/samplemod) application structure.

### Application with internal-packages

In larger applications where one or more internal packages are tied together or provide specific functionality to a larger library. We will extend the project structure again to accommodate for this.

```no-highlight
sample/
├── bin/
├── docs/
│   ├── hello.rst
│   └── world.rst
├── sample/
│   ├── __init__.py
│   ├── __main__.py
│   ├── app.py
│   ├── hello/
│   │   ├── __init__.py
│   │   ├── hello.py
│   │   └── helpers.py
│   ├── resources/
│   └── world/
│       ├── __init__.py
│       ├── helpers.py
│       └── world.py
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
├── README.rst
├── requirements.txt
└── setup.py
```

The top-level files remain largely the same as in the previous layout. These three layouts should cover most use cases for command-line applications.

## Machine Learning projects

Like our other samples, having a structured layout is useful for organising ML projects.

```no-highlight
sample/
├── data/
│   ├── external/
│   ├── interim/
│   ├── processed/
│   └── raw/
├── models/
├── notebooks/
├── sample/
│   ├── data/
│   ├── features/
│   └── model/
├── tests/
├── .gitignore
├── LICENSE
├── README.rst
├── requirements.txt
└── setup.py
```

This project structure is logical, reasonably standardised, and flexible for doing and sharing data science work.

* **data/raw**: Having a local copy of our data ensures that we have a static dataset to perform task on. Additionally, this overcomes any workflow breakdowns due to network latency issues. If there is no external data then this is the data to be downloaded by the script in `sample\data`. This directory should be considered as read only.
* **data/processed**: Data after whole preprocessing, merging, cleaning, and feature engineering.
* **data/interim**: In the event external data being available, this data would be the data that we would load for feature engineering by using a script in the `sample/data`. This dataset is generated by performing various joins and/or merges to combine the external and raw data. In short, data that is not raw but also not ready yet.
* **data/external**: This is data extracted from third party sources (Immutable data). If no third party data is extracted then this folder is obsolete.
* **models/**: We use a script in `sample\models` for training of our Machine Learning model. We may need to restore or reuse the model with other models to build an ensemble or to compare and we may decide upon a model that we want to deploy. In order to do this we save the trained model to a file, usually a pickle format, and that file would be saved in this directory.
* **notebooks**: Jupyter notebooks are excellent for prototyping, exploring and communicating findings, however they are not very good for long-term growth and can be less effective for reproducibility. The notebooks directory can be further divided into sub-folders such as `notebooks\explorations` and `notebooks\poc`. Using a good naming conventions helps to distinguish what populates each notebook — a useful template is `<step>-<user>-<description>.ipynb`, where the step serves as an ordering mechanism, the creator’s first name initial, and first 2 letters of surname and description of what the notebook contains.
* **sample/data**: In this directory we have the scripts that ingest the data from wherever it is being generated and transform that data so that it is in a state that further feature engineering can take place.
* **sample/features**: In this directory, we have a script that manipulates the data and puts it in a format that can be consumed by our machine learning model.
* **sample/models**: Contains scripts that are used to build and train our model.

## Web applications

Another major use case of Python is web applications. [Django](https://www.djangoproject.com/) and [Flask](https://flask.palletsprojects.com/en/1.1.x/) are arguably the most popular web frameworks for Python and are thankfully are a little more concrete when it comes to project structure.

### Django

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. When you [create a new Django project](https://docs.djangoproject.com/en/3.1/intro/tutorial01/#creating-a-project), it will create a directory in your current working directory with the following structure.

```no-highlight
mysite/
├── mysite/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

You’ll need to avoid naming projects after built-in Python or Django components. In particular, this means you should avoid using names like django (which will conflict with Django itself) or test (which conflicts with a built-in Python package).

### Flask

Flask is a Python web “microframework.” One of the main selling points is that it is very quick to set up with minimal overhead.

The Flask documentation has suggested layout for their [tutorial project](https://flask.palletsprojects.com/en/1.1.x/tutorial/) - a blogging application called [Flaskr](https://github.com/pallets/flask/tree/1.1.2/examples/tutorial) - let us take a brief look at the main project directory.

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

Here you can see that a Flask application, like most Python applications, is built around Python packages.

## Conclusions

Now you’ve seen example project structures for a number of different application types. Hopefully, you now have the tools to successfully prevent coder’s block by building out your application structure so that you’re not staring at a blank canvas for each and every project.

Because Python is largely non-opinionated when it comes to application layouts, you can customise these examples to better fit your use case.

Of course, there are a lot more advanced concepts and tricks we can employ in structuring a Python project but hopefully you have come away with the understanding that these rules are neither hard-and-fast. Over time and with practice, you’ll develop the ability to build and customise your own useful application layouts.

### Useful Resources

* [Sample Python Module Repository](https://github.com/navdeep-G/samplemod)
* [Pytest Docs](https://docs.pytest.org/en/latest/contents.html)
* [Python Code Quality Authority](https://github.com/PyCQA)
* [Zen of Python](https://zen-of-python.info/)

### References

1. Buschmann et al. (1996). ‘Architectural Patterns’, in Buschmann, F (ed.) Pattern-Oriented Software Architecture. John Wiley & Sons. p. 32
2. Peters, T., 2004. PEP 20 -- The Zen of Python. [online] Python.org. Available at: <https://www.python.org/dev/peps/pep-0020/> [Accessed 12 February 2021].
3. Van Rossum, G., Warsaw, B, Coghlan, N., 2001. PEP 8 -- Style Guide for Python Code. [online] Python.org. Available at: <https://www.python.org/dev/peps/pep-0008/> [Accessed 2 February 2021].
4. Bednarski, M., 2017. Structure and automated workflow for a machine learning project — part 1. [online] Medium. Available at: <https://towardsdatascience.com/structure-and-automated-workflow-for-a-machine-learning-project-2fa30d661c1e> [Accessed 1 February 2021].

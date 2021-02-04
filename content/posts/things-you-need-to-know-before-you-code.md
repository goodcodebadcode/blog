---
aliases: "/things-you-need-to-know-before-you-code"
comments: true
cover:
  alt: "Image for post cover"
  caption: "Photo by Paul Teysen on Unsplash"
  image: "/posts/images/things-you-need-to-know-before-you-code.jpg"
  relative: false
date: 2021-01-25T14:07:45.000+00:00
description: "A gentler introduction to coding"
disableShare: false
draft: true
hideMeta: false
showToc: false
tags: [  "beginners" ]
title: "Things you need to know before you code"
tocOpen: false
---

This blog post introduces you to the practicalities of web development. Let us start by looking at some computer basics and how the web works at a high-level.

Understanding the web can be intimidating and in a lot of cases confusing. Let us try and break this down into manageable pieces by starting with the basics.

> We will cover language basics in part 2.

## What is a computer?

A **computer** is a machine that performs processes, calculations and operations based on instructions provided by an end-user through software. It has the ability to accept  **input**, process it, and then produce **outputs**.

A **computer** is a machine that can be instructed to carry out sequences of arithmetic or logical operations automatically via **computer** programming.

Modern **computers** have the ability to follow generalised sets of operations, called programs. These programs enable **computers** to perform an extremely wide range of tasks.

### How does a computer work?

### What is an operating system?

### Operating systems today

### Desktop Software

### Software vs Hardware

## The Internet

### The history of the internet

### The anatomy of the internet

### How does the internet work?

Whether you are checking emails, updating your status on social media, or using watching Netflix, you are using the internet (a.k.a the web). The moment you type a web address into your browser and you hit ENTER, the following occurs:

1. The entered **URL** is resolved
2. A **request** is sent to a **server** that hosts the website
3. The **response** from the server is parsed by your **browser**
4. The webpage is **rendered** and displayed.

There are many other steps along the way, but for brevity we shall ignore these.

![](/posts/images/how_the_web_works.png)

I know that you are probably saying, “OK, I don’t understand any of that!”, so let us break this down.

### The entered URL is resolved

You opened a browser, this could be Chrome or Firefox or a number of any alternatives, and entered a web address (salesforce.com) - the browser parses the information contained in the URL.

![](/posts/images/url_parts.png)

> Sometimes you might enter an address like `www.goodcodebadcode.dev/posts`. Here the `www` is a sub-domain that just redirects to the main page. `goodcodebadcode.dev` is the domain and the `/posts` is the path. All together these parts make up the URL (the Uniform Resource Locator).

Now, it should hopefully be obvious, but the website you are attempting to browse is not stored on your machine, but instead needs to be fetched from another computer on the internet, where it is stored. This other computer is called a **server**. Why a server? Well it serves us the website content.

The web address, also called a **domain name**, is simply a human readable pointer to the address of the server that stores the content of the website. Real web addresses are not nice and are not that memorable. A real web address is nothing more than a series of numbers that typically look like this: 172.52.102.3. This is called an **IP address** and represents a unique location
on the internet, much in the same way that your phone number is unique to you.

So when you hit ENTER, the browser sends a **request** to the **server** with the **domain name** you entered. The **DNS Server** (where DNS stands for **Domain Name System**), takes the human friendly domain name and does a lookup of the IP address associated with the domain. In fact there are multiple queries and replies here but I’ll skip this for ease. Think of this process as being similar to looking through your address book (the DNS Server) to find the postal address (the IP address) for a contact.

The request is then forwarded onto the **web server** which stores the website content.

![](/posts/images/dns_resolve_url.png)

So how does the browser know about the DNS servers or their addresses - well these are programmed into the browser.

We can now move onto Step 2.

### A request is sent to a server that hosts the website

Now that we know the location of the web server that stores the content of the website. The browsers makes the request to the web server for the page we are looking for.

How is this data transferred to the server? Well, this is done using **HTTP (Hypertext Transfer Protocol)**. For many people HTTP is four letters and can be baffling but I will try to explain some of the main things you should know about HTTP.

HTTP is a protocol (the P part of the acronym). In the same way that royal protocol dictates what you should adhere to when visiting the queen - protocols are the manners or conventions that computers follow when communicating and interacting with each other. HTTP belongs to the family of network protocols and is a defined standard for what a **request** and **response** should look like. Making HTTP a standard removes the guess work for how a request has to be read by the server.

A **request** is nothing more than a data package that contains some very specific information (What is the exact URL? Which kind of request should be made? Is any metadata attached to the request?). HTTP is just a small piece in a very complex model of layers and protocols. The purpose of HTTP (which is the second T in the acronym) is to transfer this information to the server.

![](/posts/images/chrome_dev_tools_requests.png)

Lastly, the _Hypertext_ (the HT part) is what we are transferring. In short: _HTTP is the convention for how hypertext gets transferred in a network_.

> Believe it or not but your computer (or smartphone, iPhone/iPad, tablet etc.) has the hypertext transform protocol installed. Together with other network protocols which is known as the IP stack, the Internet Protocol Suite or the TCP/IP model or architecture. As a user you don't have to worry about this because the web browser of your choice does all the work and deals with it. The browser sends a request to the IP stack which does all the rest.

Once the web server receives and subsequently handles the request, a **response** is sent back. A response can be thought of as a request but in the opposite direction. Like a request, the response contains the code that is required to render the page on a screen. That response can contain any data, including images and files - this is up to the developers.

![](/posts/images/chrome_dev_tools_response.png)

### The response from the server is displayed

Your browser takes the response from the server and then parses through it doing a full head to toe scan looking for other assets that are listed, such as images, CSS files, JavaScript files, etc. For each asset it finds, the browser repeats the entire process above, making additional HTTP requests to the server for each resource.

Once your browser has finished loading all the resources that were listed in the response, the page will be loaded in the browser window and the connection to the server will be closed.

### How do browsers work?

### How does mobile internet work?

### The anatomy of a website

### The anatomy of a mobile site

### Let’s talk about Netscape

## Frontend, Backend and Full Stack

* [Frontend vs Backend vs Full-stack](https://www.youtube.com/watch?v=pkdgVYehiTE)
* [The Basics of Web Development](https://www.youtube.com/watch?v=ysEN5RaKOlA)
* What is Frontend vs. Backend?
* What do I mean by language?
* [What are the different types of Programming Languages?](https://www.youtube.com/watch?v=SBXfP3UQ7N8)
* Let’s talk about Backend
* [Finding help with problems](https://www.youtube.com/watch?v=WKuNWrxuJ9g)
* Different programming languages
* [What Programming Language Should I Learn First?](https://www.youtube.com/watch?v=poJfwre2PIs)
* What is a Tech Stack
* Common stacks for web

## Core Concepts of Coding

* Text Editors vs IDEs
* VS Code
* Syntax
* Variables
* Printing
* Commenting
* Strings
* Arrays
* Scaling: Vertical vs Horizontal
* [Programming paradigms](https://www.youtube.com/watch?v=cUjieVW7IgQ)
* [Declarative Programming](https://www.youtube.com/watch?v=yGh0bjzj4IQ)
* [Imperative vs. Declarative Programming](https://www.youtube.com/watch?v=JqvMTwnbhnA)
* [Functional Programming](https://www.youtube.com/watch?v=dAPL7MQGjyM)
* Object-Orientated Programming (OOP)
* [LOL](https://www.youtube.com/watch?v=8wUOUmeulNs)
* [DRY, KISS, YAGNI](https://www.youtube.com/watch?v=4qPYWBHkS4w)
* [Debugging](https://youtu.be/NTaNksV-DPY)

## Frameworks and APIs

* [What are libraries and frameworks?](https://www.youtube.com/watch?v=LimOOe6I4eo)
* What is a framework?
* Frontend Frameworks
* Backend Frameworks
* What is an IDE? How is this different?
* Libraries
* What is an API?

## Git and Version Control

* Git
* Continuous Integration (CI)
* [Velocity 09: John Allspaw and Paul Hammond, "10+ Deploys Per day”](https://www.youtube.com/watch?v=LdOe18KhtT4)

## How do we build software?

* Pair programming
* [How to Pair Program](https://www.youtube.com/watch?v=YhV4TaZaB84)
* [Code reviews](https://www.youtube.com/watch?v=EyL7mqwpZhk)
* [Does pair programming actually save time?](https://www.youtube.com/watch?v=G4CMZL_52rs)
* Agile
* DevOps
* [Software Development Lifecycle (SDLC)](https://www.youtube.com/watch?v=gNmrGZSGK1k)
* [Software Testing Lifecycle (STLC)](https://www.youtube.com/watch?v=goaZTAzsLMk)

## The Cloud

* SaaS, PaaS and IaaS
* [What is Cloud Computing?](https://www.youtube.com/watch?v=kQnNd-DyrpA)
* [Explaining SaaS, PaaS, and IaaS](https://www.youtube.com/watch?v=b11kNOlhfms)
* [Public Cloud vs Private Cloud vs Hybrid Cloud](https://www.youtube.com/watch?v=3WIJ4axzFlU)
* [Introduction To Amazon Web Services](https://www.youtube.com/watch?v=PW-V-72MJNY)
* [Introduction to Heroku?](https://www.youtube.com/watch?v=QTOkqzCTGxw)
* [What is Azure?](https://www.youtube.com/watch?v=3Arj5zlUPG4)
* [AWS vs Azure](https://www.youtube.com/watch?v=xCabPpcq8Ac)

## Mobile Development

* Hybrid apps
* RWD
* Mobile development
* SWIFT

## Next steps
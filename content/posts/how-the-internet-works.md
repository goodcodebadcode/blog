---
title: "How the internet works"
description: “A complete beginners guide to JavaScript.”
date: 2021-01-18T12:40:27+00:00
draft: true
aliases: ["/how-the-internet-works"]
tags: [ beginners, webdev, javascript ]
hideMeta: false
disableShare: false
showToc: true
tocOpen: true
cover:
    image: "/posts/images/laptop_and_phone.jpg"
    alt: "Image for post cover"
    caption: "Photo by Sigmund on Unsplash"
    relative: false
comments: true
---

This blog post introduces you to the practicalities of web development. Let us 
start by looking at how the web works at a high-level.

> Language basics crash course in part 2.

Understanding the web can be intimidating and in a lot of cases confusing. Let 
us try and break this down into manageable pieces.

## How does the internet work?

Whether you are checking emails, updating your status on social media, or using 
watching Netflix, you are using the internet (a.k.a the web). The moment you 
type a web address into your browser and you hit ENTER, the following occurs:

1. The entered **URL** is resolved
2. A **request** is sent to a **server** that hosts the website
3. The **response** from the server is parsed by your **browser**
4. The webpage is **rendered** and displayed.

There are many other steps along the way, but for brevity we shall ignore these.

![](/posts/images/how_the_web_works.png)

I know that you are probably saying, “OK, I don’t understand any of that!”, so 
let us break this down.

**Step 1: The entered URL is resolved**

You opened a browser, this could be Chrome or Firefox or a number of any 
alternatives, and entered a web address (salesforce.com) - the browser then 
parses the information contained in the URL. 

![](/posts/images/url.png)

> Sometimes you might enter an address like www.salesforce.com/products/analytics/. 
> Here the ```www``` is a sub-domain that just redirects to the main page. 
> ```salesforce.com``` is the domain and the ```/products/analytics``` is the 
> path. All together these parts make up the **URL** (the Uniform Resource 
> Locator).

Now, it should hopefully be obvious, but the website you are attempting to 
browse is not stored on your machine, but instead needs to be fetched from 
another computer on the internet, where it is stored. This other computer is 
called a **server**. Why a server? Well it serves us the website content.

The web address, also called a **domain name**, is simply a human readable 
pointer to the address of the server that stores the content of the website. This 
address is called an **IP address** (an IP address typically looks like this 
172.52.102.3).

So when you hit ENTER, the **browser** sends a request to the server with the 
**domain name** you entered. The **DNS Server** (where DNS stands for Domain 
Name System), takes the human friendly domain name and looks up the **IP address** 
associated with the domain. This is similar to looking through the Yellow Pages 
(the DNS Server) to find the street address (the IP address) for Salesforce Inc. 
Think of the Domain Name System as nothing more than a big address book.

The **request** is then forwarded onto the web server which stores the website 
content.

So how does the browser know about the DNS servers or their addresses - well 
these are programmed into the browser.

![](/posts/images/resolving_the_url.png)

**Step 2: A request is sent to a server that hosts the website**

Now that the IP address has been resolved i.e. we know the location of the web 
server that stores the content of the website. The browsers makes the request to 
the web server for the page we are looking for.

 A **request** is a real technical “thing” and a lot happens behind the scenes. 
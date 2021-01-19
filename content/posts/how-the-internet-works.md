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

### The entered URL is resolved

You opened a browser, this could be Chrome or Firefox or a number of any 
alternatives, and entered a web address (salesforce.com) - the browser then 
parses the information contained in the URL. 

![](/posts/images/url_parts.png)

> Sometimes you might enter an address like ```www.goodcodebadcode.dev/posts```. 
> Here the ```www``` is a sub-domain that just redirects to the main page. 
> ```goodcodebadcode.dev``` is the domain and the ```/posts``` is the path. All 
> together these parts make up the URL (the Uniform Resource Locator).

Now, it should hopefully be obvious, but the website you are attempting to 
browse is not stored on your machine, but instead needs to be fetched from 
another computer on the internet, where it is stored. This other computer is 
called a **server**. Why a server? Well it serves us the website content.

The web address, also called a **domain name**, is simply a human readable 
pointer to the address of the server that stores the content of the website. 
Real web addresses are not nice and are not that memorable. A real web address 
is nothing more than a series of numbers that typically look like this: 
172.52.102.3. This is called an **IP address** and represents a unique location 
on the internet, much in the same way that your phone number is unique to you.

So when you hit ENTER, the browser sends a **request** to the **server** with 
the **domain name** you entered. The **DNS Server** (where DNS stands for **Domain 
Name System**), takes the human friendly domain name and does a lookup of the IP 
address associated with the domain. In fact there are multiple queries and 
replies here but I’ll skip this for ease. Think of this process as being similar 
to looking through your address book (the DNS Server) to find the postal address 
(the IP address) for a contact.

The request is then forwarded onto the **web server** which stores the website 
content.

![](/posts/images/dns_resolve_url.png)

So how does the browser know about the DNS servers or their addresses - well 
these are programmed into the browser.

We can now move onto Step 2.

### A request is sent to a server that hosts the website

Now that we know the location of the web server that stores the content of the 
website. The browsers makes the request to the web server for the page we are 
looking for.

How is this data transferred to the server? Well, this is done using **HTTP 
(Hypertext Transfer Protocol)**. For many people HTTP is four letters and can be 
baffling but I will try to explain some of the main things you should know about 
HTTP.

HTTP is a protocol (the P part of the acronym). In the same way that royal 
protocol dictates what you should adhere to when visiting the queen - protocols 
are the manners or conventions that computers follow when communicating and 
interacting with each other. HTTP belongs to the family of network protocols and 
is a defined standard for what a **request** and **response** should look like. 
Making HTTP a standard removes the guess work for how a request has to be read by 
the server.

A **request** is nothing more than a data package that contains some very specific 
information (What is the exact URL? Which kind of request should be made? Is any 
metadata attached to the request?). HTTP is just a small piece in a very complex 
model of layers and protocols. The purpose of HTTP (which is the second T in the 
acronym) is to transfer this information to the server.

![](/posts/images/chrome_dev_tools_requests.png)

Lastly, the _Hypertext_ (the HT part) is what we are transferring. In short: 
_HTTP is the convention for how hypertext gets transferred in a network_. 

> Believe it or not but your computer (or smartphone, iPhone/iPad, tablet etc.) 
> has the hypertext transform protocol installed. Together with other network 
> protocols which is known as the IP stack, the Internet Protocol Suite or the 
> TCP/IP model or architecture. As a user you don't have to worry about this 
> because the web browser of your choice does all the work and deals with it. 
> The browser sends a request to the IP stack which does all the rest.

Once the web server receives and subsequently handles the request, a 
**response** is sent back. A response can be thought of as a request but in the 
opposite direction. Like a request, the response contains the code that is 
required to render the page on a screen. That response can contain any data, 
including images and files - this is up to the developers.

![](/posts/images/chrome_dev_tools_response.png)

### The response from the server is parsed by your browser and displayed

Your browser takes the response from the server and then parses through it 
doing a full head to toe scan looking for other assets that are listed, such as 
images, CSS files, JavaScript files, etc. For each asset it finds, the browser 
repeats the entire process above, making additional HTTP requests to the server 
for each resource. 

Once your browser has finished loading all the resources that were listed in the 
response, the page will be loaded in the browser window and the connection to 
the server will be closed.
---
aliases: "/what-is-clean-code"
comments: true
cover:
  alt: "Image for post cover"
  image: "/posts/images/what-is-clean-code.jpg"
  relative: false
date: 2021-01-21T13:24:18.000+00:00
description: "Writing clean, understandable, and maintainable code is a skill that is crucial for every developer to master."
disableShare: false
draft: false
hideMeta: false
showToc: false
tags: [  "clean-code", "best-practices" ]
title: "What is Clean Code?"
tocOpen: false
---

As developers, we rarely have the time to perfect a single piece of code. The idea of **perfect code is a fantasy**, and everything we write has flaws. Yet, we should always endeavour to produce the best possible code given our constraints. 

These constrains come in the form of deadlines, business demands, and personal fatigue. So, whilst different business conditions may set the boundaries on what is “possible”, a developer always has some control over the quality of his or her work.

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." - Martin Fowler 

## The cost of writing bad code

The bottom line is that code that “works” is not always “done”. If our primary focus is to produce a product that solves a business problem, then does it really matter how the problem is solved? Yes. Clients, users, companies, and other developers all benefit in the long term. 

Code that is messy, rushed, or just “good enough” is code with an eye on short-term returns.

Messy code is harder to maintain and slows you down. The more developers that work on that mess, the slower we all become over time.

![](/posts/images/productivity-decline.gif)

Seeing the decrease in productivity, management then inevitably add more resources to work on the mess, believing this will speed things up. But ultimately, it makes things worse. This observation within the industry is not new and was first attributed to [Frederick Brooks](https://en.wikipedia.org/wiki/Fred_Brooks), and is aptly named [Brooks's law](https://en.wikipedia.org/wiki/Brooks%27s_law).

According to Brooks, in his seminal book [The Mythical Man Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month), an incremental person, when added to a project, makes it take more, not less time.

So why should you care?

If you write clean code, then you are helping your future self and your co-workers. You are reducing the cost of maintenance of the application you are writing. You are making it easier to estimate the time needed for new features. You are making it easier to fix bugs. You are making it more enjoyable to work on the code for many years to come. Essentially you are making the life easier for everyone involved in the project.

## Writing good code is hard

According to the literally hundreds of books and online courses claiming they can teach you to code in two weeks, ten days, or even twenty-four hours - writing code is easy.

The problem with code is that good code, code that serves its purpose, has little or no defects, can survive and perform it’s purpose for a long time, and is easy to maintain, is very difficult to accomplish.

> It’s not just the technology; it’s the code that makes software powerful.

Like the second law of thermodynamics, disorder in a system will always increase unless you spend energy and work to keep it from increasing. Writing clean code is hard work. It takes a lot of practice and focus during execution.

To be able to write clean code you should train your mind over a period of time. The hardest part is simply making a start, but as time goes by and your skill set improves, it becomes easier.

## Software Craft(wo)manship

In recent years, we have witnessed the advent of Agile in companies, which allows us to deliver the right products to customers by significantly reducing feedback loops.

**But what about the quality of the deliverables?** The [Agile Manifesto](https://agilemanifesto.org/) does not mention anything about it. With frameworks like Scrum, we have long focused on delivering more value to our customers. But, something got lost on the way by focusing on methods, frameworks and processes. **What about the software engineers and the quality they are supposed to guarantee?**

This growing movement within the industry, mainly amongst [Agile Software Development](https://en.wikipedia.org/wiki/Agile_software_development) teams, focuses on raising the bar on the professional development of individuals by practicing [Software Crafts(wo)menship](https://manifesto.softwarecraftsmanship.org/) and subsequently helping others to learn the craft.

This can be seen as a response by software developers to the perceived ills of the mainstream software industry, including the prioritisation of financial concerns over developer accountability.

![](/posts/images/software-craftsmanship.jpg)

So how does this relate to clean code? Software crafts(wo)manship, means caring not just what software does, but how it’s built. It is about taking certain guiding principles into consideration when programming. It is less about having concrete instructions on what to program in a specific situation and more about reflecting on one’s own development work practices.

It is a style of creating code in which the software developer works in a community, and as a partner.

These practices and principles, mostly from the [eXtreme Programming (XP)](https://en.wikipedia.org/wiki/Extreme_programming) method, such as Test Driven Development, Pair Programming, Refactoring, Continuous Integration, etc. allow us to create software of higher quality.

## Clean Code

As Robert C. Martin stated in his book [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), “Clean code is code that has been taken care of. Someone has taken the time to keep it simple and orderly. They have paid appropriate attention to details. They have cared.”

The most popular definition of clean code is **code that is easy to understand and easy to change**.

The problem with this statement is that is states something without really stating anything at all. Clean code is subjective and every developer has a personal take on it. There are some ideas that are considered best practice and what constitutes as clean code within the industry and community, but there is no definitive distinction — indeed, even Martin’s book begins with a collection of definitions given by various software luminaries.

So since there is not definitive definition, let us revisit that earlier version.

### Easy to understand

> As [Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum) said, “Code is read much more often than it is written.” You may spend a few minutes, or a whole day, writing a piece of code to process user authentication. Once you’ve written it, you’re never going to write it again. But you’ll definitely have to read it again. That piece of code might remain part of a project you’re working on. Every time you go back to that file, you’ll have to remember what that code does and why you wrote it, so readability matters.

Easy to understand means the code is easy to read, whether that reader is the original author of the code or somebody else. It’s meaning is clear so it minimises the need for guesswork and possibility for misunderstandings. It is easy to understand on every level, specifically:

- It is easy to understand the execution flow of the entire application
- It is easy to understand how the different objects collaborate with each other
- It is easy to understand the role and responsibility of each class
- It is easy to understand what each method does
- It is easy to understand what is the purpose of each expression and variable

**The real test of readable code is to have other developers read your code. So get feedback from others, via code reviews.**

### Easy to change

Easy to change means the code is easy to extend and refactor, and it’s easy to fix bugs in the codebase. This can be achieved if the person making the changes understands the code and also feels confident that the changes introduced in the code do not break any existing functionality. For the code to be easy to change:

- Classes and methods are small and only have single responsibility
- Classes have clear and concise public APIs
- Classes and methods are predictable and work as expected
- The code is easily testable and has unit tests (or it is easy to write the tests)
- Tests are easy to understand and easy to change

## Conclusion

Clean coding is not a skill that can be acquired overnight. It is a habit that needs to be developed by keeping these principles in mind and applying them whenever you write code. The bottom line is that clean code is in the eye of the beholder. 

Over the next few months, I will write a series of posts where we look at clean code ideas in more detail. Until then, take care.

### Useful Resources

### References

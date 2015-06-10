---
layout: post
title: "Explaining Dependency Injection to an Eight Year Old."
author:
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "That kid is in for a tough life."
---

### Introduction

Dependency Injection (DI) is a very fancy name for what is essentially a rather simple concept.

As James Shore puts it :

| Dependency Injection is a $25 term for a 5c concept.

I like simple. It's easy to remember, hell it's easy to *draw*. I like to draw conceptual things (like software design), it makes them real.

Simple can be hard to achieve however. I feel like the Internet has done a spectacular job of confusing me about Dependency Injection (DI).

I felt uncomfortable writing DI code from day one. I was doing it, but I wasn't truely sure why. In these moments of obscurity - i need an elivator pitch. I need it trivialized. I need you to explain it to me like I'm 8 years old.

Most people struggled to explain it to me when I asked them about it. Why is everybody so vague on the details? Why do programmers squirm around with weak analogies when I ask them to teach me?

It's actually very simple, but I feel certain things contribute to the fear of this black-magic pattern.

+ The name.
+ Libraries that expose it as a tool can often look foriegn. (such as Guice or Dagger in Java).
+ Existing DI code can be hard to read or understand, or even trace properly in an IDE.
+ It can be hard to appreciate until you face the problems it helps solve.
+ Lots of important people write lots of big blogs about the numerous variations of the pattern.

Software is already complex, don't make it any worse people.

This is my attempt to explain what Dependency Injection is, how it looks, and why it exists.

---

### Big Picture

(explain DI 'graph' )

Dependency injection means *giving* an object its instance variables.  
That is to say, we **don't** let it define its **own** instance variables.

That's pretty much it.

An Object, really just comprises of two things.

+ Values. These are the *variables*. The Nouns.
+ Actions. These are the *methodss*. The Verbs.

You can think of an Object's variables as its "dependencies". These are the things that make it what it is. It depends on them.

Normal Way.

{}



DI Way.

{}


See? That's it. Providing our own values for classes, passing them in. This is **injecting** the **dependency**.


---

### An Example

Building a rocket ship.



---

### An Example with Dagger or Guice.


---

### Why Use It?

Testing.

---

### Tools

---

### Further Reading

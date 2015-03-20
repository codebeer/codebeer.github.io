---
layout: post
title: "How to make autocorrect amazing"
permalink: autocorrect-idea
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradley_curran"
excerpt: "Have you ever sent the text \"OMG autocorrect\"?"
---
Autocorrect in its current form is already amazing. It's a great feat of engineering that merges user experience and modern technology. It's so good that we don't register that it's doing it's thing most of the time. 

But when it fails, when we send that ridiculous autocorrected word, that's when we recognise that technology is getting in our way. 

![Autocorrect](/images/posts/autocorrect/auto1.png)

The thing is, autocorrect puts the wrong word in all the time. But we go back and write the right word. The real problem is when someone sends a text with the last word autocorrected. See: above. 

What's a solution? I think this one is simple conceptually but harder to pull off from a UX perspective. 

<blockquote>
    When a user sends a text and the last word was autocorrected, give them a short time after hitting Send before actually sending the text. 
</blockquote>

This gives the user the ability to cancel the text and correct the word sent. 

A timer is displayed next to the text that counts down five seconds or so. If the user taps the text in that time, the device never sends the text and repopulates the text field with the cancelled text. 

There's a few issues that need to be resolved with this: 

- **Negative UX**: The user has this timer displayed with a countdown. This is going to stress out the user. Sending a text is going to be associated with stress. 
- **Complexity**: Most chat and text systems are very simple. Adding this kind of functionality may make the user feel alienated and confused. 

Originally I thought it would be cool to do something a lot more simpler: 

<blockquote>
    If a user types a text with the word "autocorrect" in it immediately after another text that has the last word autocorrected, then reduce the chance this word will be autocorrected in the future."
</blockquote>

This is more behind the scenes, where the technology should be. My biggest issue with this was that I don't know if this is even possible with current autocorrect technology. 

Turns out a lot of people don't write "OMG autocorrect" immediately after sending a ridiculous autocorrected text like me though. :)

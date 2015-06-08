---
layout: post
title: "Type Holy Wars (Flame On! Edition)"
author:
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "Have static languages ruined me for life? Yeah, probably."
---

## Taking the Boy out of the Java

When I started my new Job at REA, I started with an outspoken goal to add Ruby
as a core language to my toolbelt.  

My entire academic career and my two professional coding years have been spent
using Statically Typed languages. Predominantly Java, Objective C and C89.

In my short career, I had met a handful of characters who I admired or
who had in some way peaked my professional curiosity. The common attribute that
tied these people together was their proficiency (read: love affair) with Ruby.

These people knew something I didn't. They were not just TDD/BDD users, but advocates.
They had a pattern of problem solving that was visibly different to mine. They had a particular
appreciation for code that I felt I lacked.

They had beards. They had Fixed Gear Bikes. They had Rails.

I wanted that.

## Taking the Java out of the Boy

I've been wrestling with Ruby in a professional environment for the past 2 weeks,
mostly observing in a pair with another developer whilst taking breaks
to press my fingers into my eye sockets and figuratively sponge my brains off the walls.  

---

## The Devil's Advocate

### But Java is **awful**! Remember? Matt?

People who think Java is *awful* tend to be the same people who are *awful* at writing Java.
You need to make a clear distinction between these two things :

+ Java, the Language.
+ Java, the programmers.

There is a particular demographic of developer that have saturated the current Java
community due to a number of environmental factors.
Primarily, the historical use of Java in the corporate enterprise space.

I know the programmer you're thinking of, I'm not them.

---

### I can write 100 lines of Java, in 10 lines of Ruby.

Yes, Ruby is concise, Java is verbose. I understand that.
Admittedly for certain tasks there can be a bit of 'boilerplate' overhead when writing Java.

However this **verbosity** gives me safety. It gives me information on the glass that
actually tells me important details about the code. Ruby's level of brevity often leaves
me with more questions than answers.

I was *Ping-Pong-Pairing* on Friday writing my first Rspec test. My pair wrote
a failing test, and it was my task to make it pass. It was as simple as creating and returning
a Model. Here was the conversation between the Computer and I.

Where are the inbound parameters I use to populate this model?  
**>** *They're hidden in the single options Hash passed in, figure out the keys yourself moron.*  
How do I know what parameters the Model itself takes to be created? I guess I'll check the Class definition.  
**>** *Nope. This model inherits from ActiveRecordBase, so it's literally empty.*  
Okay fine, I'll change windows, start an irb REPL and construct a blank
instance to get the parameter list.  
**>** *Okay*  
Now at least have the damn key names for this Hash. Let's do this.  
**>** *EXPLODE - NOPE WRONG TYPES MATT*  

![rage][rage-gif-1]  

The explicit text that came with Java was giving me useful information.
I can't do any better than quote @Joneaves on this one. (and I paraphrase)

> If physically typing out each laborious character on your keyboard is the hardest part of programming for you,
> you've got bigger fucking problems.

---

### Ruby reads so beautifully!

I'm sure it would be a pleasure to read if I could ever fucking find it.
One of the biggest pains I'm feeling having come from Static Typing is the
sudden lack of *discoverability*.
Who uses this Class? How do they use? On what grounds do they feel they can use it?
All a mystery. But why do you care? They're all *ducks* right?

There is no `Type Hierarchy` to traverse, there are no formal contracts between components
 (ie: Java's `interface`).
(Honestly, I even have to keep reminding myself there *is no* Compile Time. )
Want to bolt on behavior that doesn't belong to this class? Go ahead. People overload `method_missing` all the time.
Are those pesky `nils` popping up when you don't want them? Just bolt responding methods onto the NilClass, it's a pattern.

This flexibility makes it very difficult for me coming into a mature codebase to
traverse the flow of code or identify relationships between classes.

Java's interfaces provided a contract for components to conform to and
provide a certainty of behavior.

It gave me a clear definition of responsibilities at the top of my files,
it gave me unification, hell it gave me autocomplete  - but ultimately
it made me *really think* about how I was modeling the world in my software.

---

### I don't think you appreciate truly how beautiful it is Matt.

Because Ruby is *just text* as I like to say, admittedly it is super easy to reach in
and start creating very readable code by creating your own DSLs.

Unfortunately everyone seems to have picked up on this.

It might be making your life easier, but as a new comer to the language, even just reading the
most simple codebases means having to read, learn and segregate several flipping DSLs in the one project.
(Rspec, Rails etc etc). Not to mention that this seems to be a cultural problem when I find people **frequently**
not understanding the language they get paid to work with because they don't know where Ruby ends and the DSL begins.

![dsl][dsl]

You've actually made my job of on-boarding more difficult.

---

### At least Ruby won't cripple you with your own Class Hierarchy.

True. The very benefits I listed above are also, nine times out of ten
- the downfall of a Java project.
The strong static typing coupled with a short-sighted app architecture
can cripple a codebase's ability to change. Then you start hacking, then you
realize your hacking your own hacks - and you question your career choice.

Ruby wants to free you of these bindings. Ruby wants to be a pleasure to refactor.

Ruby's answer to this is Duck Typing.

> If it looks like a Duck, and quacks like a Duck - It's a Duck.

A senior engineer I work with has a fantastic affectionate term for this methodology.

> Duck Punching  
> - Jay Dwyer

Well, this class was a Duck - but now I guess it's also a horse,
and an Authentication Delegate, and a Model Validator.
Not that you would ever even know, unless you've memorized the corresponding
method signatures. Did you? No. Didn't think so.

---

## In Closing

![rage][rage-gif-2]

[rage-gif-1]: /images/posts/ruby/rage1.gif
[rage-gif-2]: /images/posts/ruby/rage2.gif
[dsl]: /images/posts/ruby/dsl.png

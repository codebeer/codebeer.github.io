---
layout: post
title: "Matt Responds To : Booleans in Method Signatures"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "I'm having a party, but don't tell Java. I'm trying to make new friends."
---
# Prologue

At first I was very aware that the code I wrote, while adequet, was generally awful. I was comfortable with this fact. It was sort of humbling.

After more than a year working in the same framework, I returned to an old project of mine and scoffed at my desk "I was writing some pretty shit code back then". Without hesitation, a colluage's voice very casually muttered back from behind a monitor "You still are".

There was a split second of expected offense, before it was totally erased by the sobering fact that he was dead right. 

Really, it's part of why I like the job. I've learnt that to be anything more than a trivial programmer, you have to keep an absolute open mind all the time towards problem solving, process and idealology. 

I've taught hundreds of University students, many of whom think their code is gifted from God himself. However the 'Humble' students who are open to change are the ones that make the biggest progress as developers. 

Brad commends this realization as a "known unknown". The ability to critically reflect and appreciate, but then also identify, the things you know you don't yet understand. I think perfecting code style and design *can be* a never-ending "known unknown" for Developers.

Everything I write looks great at the time and disgusting even just the next day. 

---

# In Response  

I'm going to offer a few small related examples from other languages I have just started playing with this week, and how the method signature discussion changes.


## Ruby

### Default Parameters instead of Overloaded Methods  

(I'm going to start off-track and wind my way up to my point.)

Say we have the following method

{% highlight ruby %}
def build_robot(power_core, weapon)
	#...
end
{% endhighlight %}

It's likely that we will want to construct a friendly robot with *no* weapons.

`build_robot(fusion_core, nil)`

Java devs may be tempted to create an overloaded method, giving us the calls

`build_robot(fusion_core)` or

`build_robot(fusion_core, rockets)`

However in Ruby when you define a method you can supply *default parameter values*. 

{% highlight ruby %}
def build_robot(power_core, weapons = nil)
	#...
end
{% endhighlight %}

Now we can call `build_robot(fusion_core)` and the weapons field will be set to nil for us automagically.

This means Brad's examples of drawing views, could have a default (say the light view) and only change to alternative views when a parameter was included, without seperate method declarations!

---  

## Not a Solution  

This seems like a good idea at first, but you quickly create the same problems you do in Java, long parameter lists that cause you do constantly have to jump to the Class definations to double check what the bloody paramters actually are.

Consider the following method with a long list of input parameters...
{% highlight ruby %}
def build_robot(power_source = nil, 
	weapons = nil, 
	logic_unit = nil,
	special_function = nil, 
	transport_mode = nil) 
{% endhighlight %}

Now even with default paramters instead of overloading - If you had a very simple **flying robot**, you might have something like. 

`build_robot(fusion_core, nil, nil, nil, jet_thrusters)`

 Agh! Kill it with fire! We still have to pad the method call with `nil`s even though we specified default values, because if we didn't, Ruby wouldn't be able to tell the `jet_thrusters` are a `transport_mode` and not a `weapon`.

![diagram with assumed nil padding][nil-pad]

**note** For this example i've used custom input types for clarity but you can just as easily imagine this as a series of booleans.

---  

## Options Hashes  

For this reason Rubyist will usually use a trailing Hash for any method argument that is non-essential, and *this* is where i get to my point.

Here, a `power_source` is the only requirement, and the method now only takes 2 inputs.

{% highlight ruby %}
def build_robot(power_source, options = {})
	
	bot = Robot.new
	bot.power_source		= power_source
	bot.weapons  			= options[:weapon]
	bot.logic_unit 			= options[:logic]
	bot.special_function 		= options[:special]
	bot.transport_mode 		= options[:transport]
end
{% endhighlight %}

Now when we call this method, not only can we specify **only what we're interested in**, but each paramter we **do** provide is explicitly named thanks to the key/value syntax. 

{% highlight ruby %}
build_robot(fusion_core,
	:special 	=> mustard_dispenser,
	:transport 	=> jet_thrusters)
{% endhighlight %}

**Added Bonus** Now, because it's just a Hash, the order we specify the arguments in doesn't matter! Put away your API doc!

Awesome, they're clearly named, we don't have to pad `nil`s and we don't even have to get them in the right order.

I've worked with some enterprise Java libraries. They'll have a long obscure method signiture peppered with various `null`s & `boolean`s, and have little documentation - causing this mess :

{% highlight java %}
trackEvent(this, bundle, false, null, null, true, true, false)
//haha...wut.
{% endhighlight %}

What do the booleans represent?
Are you **sure** they're in the right order?
Arn't those two `nulls` important?

Kill me now.

Now using this clearly labelled options Hash, we can get away with this heavy use of boolean parameters. (if there really wasn't another way). We specify only what we need to (default the others) and clearly label them in whatever order. Making a happy developer.

{% highlight ruby %}
track_event(self, 
	bundle,
	:withLocation	=> true,
	:onMobile	=> true)
{% endhighlight %}


--- 

## Strategy Patterns Kill Kittens  

Brad took the discussion up a notch with the suggested use of the Strategy Pattern, so now I'm going to take it waaaay off track.

> extract the functionality of drawing a view into a separate object. 
> - Brad

If you havn't already, stop everything you're doing and go read Steve Yegge's (Lengthy) blog post [**Execution in the Kingdom of Nouns**](http://steve-yegge.blogspot.com.au/2006/03/execution-in-kingdom-of-nouns.html). 

Done? Don't lie to me because I'll know. Go and read it.  
Great wasn't it? That's how I first got interested in NodeJS.  

Steve explains an outcome as a series of applied actions or *verbs*, he uses an everyday example of taking out the garbage.

> Regardless of the language you chose, or the exact steps you took, 
> taking out the garbage is a series of **actions** that terminates in the
> garbage being outside, and you being back inside, 
> because of those actions you took.
> ... Action even gives spices their spice! After all, they're not spicy until you **eat** them.

This sort of language opened my mind to the idea of psuedo-functional programming with Node. Having a series of immutable values that have functions applied to them in a series for an outcome.

At the time, I had also just watched a [Micro Services talk](http://vimeo.com/79866979) by Fred George (I can't find it, but that link is close enough) and the two concepts seemed to bleed together in my mind. 

I imagined a sort of pipe-and-filter immutable data ecosystem of rivers and streams where data could be plopped into the mountain top and have it perpetually float through a river of verbs. It could be distributed and polyglot, it could be commonly bound by [seneca.js](senecajs.org), it could be beautful!

But i digress... 

I've used the Strategy Pattern pattern myself many times, to solve Brad's problem and avoid a monolithic tree of boolean flow paths. 

But now when I do i can't help but think of Steve Yegge's blog as he talks about **The Kingdom of Nouns**.

> In Javaland, by King Java's royal decree, Verbs are owned by Nouns.  
> ...If a Verb is to be seen in public at all, it must be escorted at all times by a Noun. Of course "escort", being a Verb itself, is hardly allowed to run around naked; one must procure a VerbEscorter to facilitate the escorting...

Steve goes on to mention the Java Kingdom's citezen's love of Architecture. 

> Architecture is held in exceptionally high esteem by King Java, because architecture consists entirely of nouns. 

I feel this again throws back to the Strategy Pattern, an entire architectural mechanism employed for some simple behaviour. Behaviour facilitated and **dictated** by architecture. 

Generally in my experience, architecture means *we'll see the shortcomings of this in a few weeks time.*  

Don't get me wrong, I love the stuff - architecture gets me off - but at a design level like this - it can feel heavy handed. 

Brad's original example was drawing views, and having to maintain a set of Class files to dictate run-time functions feels like something Steve wouldn't be impressed by. Having to facilitate this `draw` verb in a **series** of `Drawer` nouns bound to a Class hierarchy, growing ever bigger as the client demands more views, praying at the Alter of the Polymorph.

Yep, sounds like Java. 

Sadly, I don't actually have a practical code example that is particularily "functional" to counter with, and I hope that in the coming days as I deep dive into both Scala and Ruby, I will have a more functional resolution that I can offer. 

I've done enough Objective-C to know however, that the answer is **not** a `BlockFactory`, that is a singleton block-vending-machine anti-pattern that you will regret. 

As it stands, irrespective of language, I would favour good method design before electing an actual design mechanism like a runtime, polymorphic Strategy Pattern. 

I did that once for generating Tweets. No joke. 

---  

# Conclusion  

In a last ditch effort to pull this back towards even remotely resembling a counter-post to Brad's original "Should booleans be used in method signatures?" I favour his notion of "More methods, smaller methods", and add that languages outside of Java offer some neat alternatives such as Ruby's options Hash. 

Also the strategy pattern should die in a fire because functions, traits etc. I guess.

[nil-pad]: /images/confused_nil_padding_ruby.png "Incorrectly assuming Ruby will understand implicit nil padding"





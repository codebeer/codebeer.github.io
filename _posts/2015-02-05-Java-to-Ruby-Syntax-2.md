---
layout: post
title: "Java to Ruby - Syntactic Sugars Part 2"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "Ruby Codecation Post #3. A brief look at some small syntax differences that makes Ruby so concise. Includes OO related syntax, using Self and more"
---

## Foreword

This belongs to a **[Java to Ruby Series][1]** of posts. Checkout that series link for a description of the series' intended audience and other posts.

---

## Introduction 

See the introduction for part one if you havn't already.
 
We already looked at very granular facets of the language, so now lets look at more interesting stuff - namely OO related syntax and gotchya's plus a brief look at Ruby's map function. I might make an in-depth about the Enumerable package in the future - but the internet has hundreds of them.  

---

## Custom Objects

Instead of same-name constructors, Ruby has the `initialize` keyword for constructing default objects.

{% highlight ruby %}
class Bear
    def initialize(name, type)
        @name = name 
        @type = type
    end
end
{% endhighlight %}

You can probably tell from this alone that Ruby also has special syntax for defining Instance variables (with the `@` character) - this saves a lot of template code you would otherwise write in Java.

## Trouble with Instance Variables

In Java it's pretty common to see pre-set instance variables for a class

{% highlight Java %}
public Class Bear{

    private String type = "Panda";
    
    public printType(){
        System.out.println(type);
    }
}
{% endhighlight %}

But looks what happens when we repeat the same in Ruby.
{% highlight ruby %}
class Bear
    @type = "Panda"
    def print_type
        puts @type
    end
end

bear = Bear.new
bear.print_type
#=> nil
puts bear.instance_variables
#=> []

{% endhighlight %}

Our values were never set! Whaaa?

An easy mantra to remember is that instance variables really don't exist until their used. They need to be a part of what I refer to as the living code (run inside methods, not the flat declarations at the top of the class). 

In Ruby you should be using the `initialize` constructor we already covered. If you assign instance variables in here, they will *spring to life* when the object is created. 

Here is an example showing what I mean.

{% highlight ruby %}
class Bear
  #..
  def bring_code_to_life
    @type = "Panda"
  end
end

bear = Bear.new
puts bear.instance_variables 
#=> []

bear.bring_code_to_life
puts bear.instance_variables
#=> ["@type"]
{% endhighlight %}

---

## Subclassing

Instead of the `extends` keyword, Ruby uses a `<` symbol. 

{% highlight ruby %}
class Bear < Animal 
    def initialize(name, type)
        super(name)
        @type = type
    end
end
{% endhighlight %}

---

## Class Methods

Inside a class, the `def` keyword creates a new instance method, when used without an explicit receiver.

In the context of a class, `self` refers to the **current** class, which is simply an instance of the class `Class`. 

Defining a method on `self` creates a class method (ie, **all** instances of that class).

{% highlight ruby %}
class Bear
  #...
  def self.print_genus      # <-- Class Method: note use of self
    puts "All Bears are from the Ursus genus!"
  end

  def print_colour          # <-- Instance Method
    puts "This specific bear is #{@colour}"
  end
end

Bear.print_genus
#=> "All Bears are from the Ursus genus!"
{% endhighlight %}

---

##Setters & Getters

The `attr_reader` method defines the 'getter' method for you, this convenient shortcut means you don't have to write any template getter code (at least if you're getter is just simply exposing the value, and not doing some special mutations).

The same method exists for setters, called `attr_writer` - this sets the value of the instance variable with the same name as the setter. 

Usually you want to expose both the getter and setter for an instance variable. You don't have to write both `attr_reader` and `attr_writer`, you can use another method, the `attr_accessor`, which will define both at once.

{% highlight ruby %}
class Bear
    attr_accessor name #same as declaring setName / getName
    def initialize(name, type)
        @name = name 
        @type = type
    end
end
{% endhighlight %}

---

## Using Self

Coming from a strongly typed world like Java often means my most puzzling problems are due to being unfamiliar with the intimate details of a loosely typed or dynamically typed language. 

Consider the below code.

{% highlight ruby %}
class Bear
    attr_accessor :name
   
    def change_name(new_name)
        name = new_name     # <-- !
    end

    def say_name
        puts name           # <-- !!
    end
end

bear = Bear.new
bear.name = "Smokey"
bear.change_name("BoBo")
bear.say_name
#=> Smokey
{% endhighlight %}

Why did it not print Bobo? Why did it remain as Smokey?

This is because inside the `change_name` scope, the existence of a `name` variable is news to Ruby - so it creates a local variable by the same name, which ultimately is not used for anything!
Our `change_name` method essentially assigns a new name to some new variable then throws it away.

If we change that line to `self.name = new_name`, then it will work.

So although you're used to `self` (or `this` in Java) being implicit for the most part - here we actually need to specify the message receiver to Ruby. It's because we fail to specify a receiver that we are graciously given this new (unexpected) instance variable. 

So if that's the case Matt, how come you didn't have to specify `self` inside the `say_name` method? Good question reader!

It's not necessary to use `self.title` explicitly for this getter, because **this time** Ruby will notice there is no local variable by that name, and now send `self` the message for us!

What!? So `self` works automagically for getters but *not* for setters? **Yes, Exactly!**

When you think about it, it makes sense. For assignment, Ruby *must* assume you want to assign to a local variable. If instead it sent the assignment message (in this case `name=`) to `self`, we would never be able to set a local variable again!

ie: Every time we wanted a new throw-away local variable in a method, the message would be sent to up `self` instead of keeping it in our method scope.

---

# The Enumerable Style

### Why is 'map' used sometimes?

Doing stuff on an array is a pleasure in Ruby.

I'll be posting about Blocks more in the future to explain the below syntax further. But just accept it for now.

{% highlight ruby %}
bears.each do |bear|
	bear.feed
end
{% endhighlight %}

Using `each` is fine, but maybe you want to save a result of those items you iterated over? Or even a subset of them?

Unlike `each`, `map` will return a value.  

{% highlight ruby %}
fed_bears = bears.map do |bear| # <-- note we get to save some fed bears! 
	bear.feed if bear.hungry?
end
{% endhighlight %}

If like me, you're coming from Java - Ruby can feel expressive but also very implicit. Java often spells things out with little to no 'Black Magic'.

For this example, remember that we don't need to use the `return` keyword explicitly like in Java. Each time our bear is fed (and **only** when fed), he will be added to the resulting fed_bears collection.

How does it explicitly know that being fed is my condition in which to add bears? 
Because that's what .feed returned when the condition was positive - it will return nil otherwise. Map will *always* return **something**, whether a condition was true or not.

Consider this *exclusively* non-bear related example.

{% highlight ruby %}
numbers = [1,2,3,4,5,6]
evens = numbers.map do |n|
	n if n.even?
end
# => [nil, 2, nil, 4, nil 6]
{% endhighlight %}

**side note** : if you wanted a neat array without the inter-mingled `nil`s, don't strip them out of the resulting array - just use `#select`

{% highlight ruby %}
numbers = [1,2,3,4,5,6]
evens = numbers.select do |n|
	n if n.even?
end
# => [2, 4, 6]
{% endhighlight %}

Now consider we add two conditions pertaining to n.

{% highlight ruby %}
ans = numbers.map do |n|
	n if n.even?
	n if n < 5
end
# => [1, 2, 3, 4, nil, nil]
{% endhighlight %}

Perhaps even though 6 is even, it is not less than 5, and so is excluded.

Wrong! Total red herring.

Map isn't magic, it just cares about that last evalution. Notice the difference if we just swap these two lines.

{% highlight ruby %}
ans = numbers.map do |n|
	n if n < 5
	n if n.even?
end
# => [nil, 2, nil, 4, nil, 6]
{% endhighlight %}

---

## Counting with a Hash

The following code is counting Bear [sightings at the Bear Safari][bearsight], eachtime adding +1 to the count for each type of bear seen.

{% highlight ruby %}
bears_spotted = ["brown", "panda", "black", "brown", "panda", "polar", "panda"]
bear_hash = {}

bears_spotted.each do |bear|
	bear_hash[bear] += 1
end

bear_hash.each do |bear, count|
	puts "#{bear}: #{count} Sightings."
end
{% endhighlight%}

**CRASH!**

`undefined method '+' for nil:NilClass (NoMethodError)`

This is because each item in the hash is `nil` by default, and being that nil is not a number, it can't simply be incrmemted numerically. It's upset with the `+= 1` bit. 

So if we set the default for all entries to be `0` instead of `nil` in our hash, we'll be sweet. 

Super easy. Call the hash constructor explicitly with zero as the input parameter. 

{% highlight ruby %}
bears_spotted = ["brown", "panda", "black", "brown", "panda", "polar", "panda"]
bear_hash = Hash.new(0) # <------ win

bears_spotted.each do |bear|
	bear_hash[bear] += 1
end

bear_hash.each do |bear, count|
	puts "#{bear}: #{count} Sightings."
end
{% endhighlight%}

`#=> brown: 2 Sightings.
#=>black: 1 Sightings.
#=>panda: 3 Sightings.
#=>polar: 1 Sightings.`

---

##Hash Ordering FYI

Ruby's Hash order is guarenteed based on the order you add them. 
This is only true for versions after 1.9.

[1]: {% post_url 2015-01-25-java-to-ruby-series-announcement %}
[bearsight]: http://www.wildlifetrails.co.uk/assets/uploads/made/Grizzly%20Bear%20Safari%20with%20Wildlife%20Trails_1100_734_84_s.jpg

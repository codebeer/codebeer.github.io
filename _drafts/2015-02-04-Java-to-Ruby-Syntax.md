---
layout: post
title: "Java to Ruby - Syntactic Sugars"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "Ruby Codecation Post #2. Ruby : A brief look at some syntax that makes Ruby so concise. Includes when to use Self, what is thruthy and how to work with class values "
---

## Foreword

This belongs to a **[Java to Ruby Series][1]** of posts. Checkout that series link for a description of the series' intended audience and other posts.

---

## Introduction 

Java was my first language, I admit that I can derive safety from volume (read : ocean of strongly typed code).
It's explicit, it's there, read it. When it's the first thing you learn, it can feel right to explicitly declare everything.

When languages like Scala come along with their simplistic automatic getters,setters and constructors - the *black magic* can make the lack of code puzzling to newcomers trying to learn.

This post is aimed at explaining some of the syntax and magic of Ruby that will help you read examples online as you're getting started coming from Java.

Ruby developers love telling you how beautiful Ruby is to read and write. How expressive it is - like a Haiku or Jim Carrey's face.   

---

## Conditions Return Values Now

Okay, let's start simple.

In Java you could be tempted to write something like the following
{% highlight java%}
if(hasHoney){
    bearState = "Happy";
}
else{
    bearState = "Grumpy";
}
return bearState;
{% endhighlight %}

No surprises here, but here's the Ruby.
{% highlight ruby %}
if has_honey
    bear_state = "Happy"
else
    bear_state = "Grumpy"
{% endhighlight %}

However, in Ruby, the `if` conditional itself **returns a value**!  
It's such a small syntax thing but it makes all the difference. 

{% highlight ruby %}
bear_state = if has_honey
    "Happy"
else
    "Grumpy"
{% endhighlight %}

Also, note the lack of any **return** keyword. The `if` and the `else` return these values implicitly.

## Multiple Return Values

Maybe you have more than just a binary outcome? Then use a case statement in the same way.

{% highlight ruby %}
bear_origin = case bear.type
  when "koala" then :australia
  when "brown" then :USA
  when "panda" then :china
end
{% endhighlight %}

---

## dotdotdots (...) are now stars (*)

**Java**
{% highlight java %}
//print_animal_colours("Bear", "brown", "black", "white");
public void printAnimalColours(String animal, String... colours);
{% endhighlight %}

**Ruby**
{% highlight ruby %}
def print_animal_colours(animal, *colours)
{% endhighlight %}

---

## Default Parameters instead of Overloaded Methods

I already mentioned this is a [previous post][2], check it out.

---

## Exceptions 

Say goodbye to the `throw, try, catch` clauses, now you have the slightly less cumbersome 

{% highlight ruby %}
raise <exception>
    #...
begin
    #<try thing>
rescue
    #<catch error>
{% endhighlight %}

Here's a simple example, checking to see first if our farmer is an officially accredited & certified [Bear Farmer (?)][bearfarmer] before we just hand over all our precious bears! 

{% highlight ruby %}
def get_bears(bear_farm)
    unless bear_farm.certified?(@farmer)
        raise CertificationException.new
    end
    bear_farm.bears
end
{% endhighlight %}
{% highlight ruby %}
begin
    bears = get_bears(bear_farm)
    rescue CertificationException
    warn "You are not a certified Bear Farmer sir!"
end
{% endhighlight %}

This also gives me a chance to mention Ruby's `unless` keyword which removes the need for if(!condition).

You should make use of it, and avoid negative `if` assessments.

This example is the **wrong** way
{% highlight ruby %}
bear_state = if !has_honey
    "Grumpy"
else
    "Happy"
{% endhighlight %}

Instead, use `unless`.
{% highlight ruby %}
bear_state = unless has_honey
    "Grumpy"
else
    "Happy"
{% endhighlight %}

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

## Boolean Gotchya's

Unlike Java, **only** NIL is falsey in Ruby.

|:------- |:------------------- |
| Condition    | Result         |
|---------|---------------------|
| **" "** | treated as “true”!  |
| **0**   | treated as “true”!  |
| **[ ]** | treated as “true”!  |
|---------|---------------------|

Example
{% highlight ruby %}
unless name.length 
    #even if name is "", length will be 0.
    # therefore, this condition will NEVER be false.
    warn "User name required"
end
{% endhighlight %}

## Ruby developers love [Short Circuit Evaluation](http://en.wikipedia.org/wiki/Short-circuit_evaluation)

###Example in Java
{% highlight java %}
if ( name != null && name.length > 8 ){
     //we prevent null pointer exceptions from the .length call 
    //by shorting out the evaluation early with a familiar null check.
} 
{% endhighlight %}

In Ruby, shorting is also commonly used for assignment. It's possible in Java too but it's often not as nice.

{% highlight ruby %}
result = nil || 1    # 1
result = 1   || nil  # 1
{% endhighlight %}

Ruby provides one of my favourite shorting tricks, *conditional assignment*

{% highlight ruby %}
user_session ||= sign_in
#only if user_session is nil, assign the return value of sign_in
{% endhighlight %}

Ruby lets you use this operator to be really concise, the following example eliminates the need for an `if` statement.

{% highlight ruby %}
def sign_in
  user_session || sign_in_user
  #if the user_session is nil, call the sign_in_user method, otherwise carry on.
end
{% endhighlight %}

---

##Array Manipulation

You can easily add one array to another by joining the two existing arrays and simultaneously removing duplicates - all with a single operator!

{% highlight ruby %}
array | other_array
{% endhighlight %}

You can append one array to another easily too. Also, this expression returns the array itself, so you can chain several appends together!

{% highlight ruby %}
array << array2 << array3
{% endhighlight %}

You can even simulate Set Intersection, this code returns a new array containing elements common only to both, with no duplicates.

{% highlight ruby %}
array & other_array
{% endhighlight %}

---

## 1.9+ Hash Syntax

Check out a [recent post][2] that explains using symbols in Hashes and also details the v1.9 *'literal'* syntax.

In short, drop the symbol's prepended colon, and swap the `=>` for a `:`.

{% highlight ruby%}
old_way  = { :symbol_key => "value"} # bad (but common online)
new_way  = {  symbol_key:   "value"} # good :)
{% endhighlight %}

---

## Loops

Given the array `arr = [1, 2, 3]`.

In Java the variables inside a `for` declaration are short lived and restricted to the scope within that loop.

{% highlight java %}
for(int i : arr){
    System.out.print(i);
}
System.out.print(i); // <-- i is null. different scope, crash.
{% endhighlight %}

**However** in Ruby for a similar loop - that's not true!

{% highlight ruby %}
for elem in arr do
  puts elem
end

puts elem # => 3
          # elem is accessible outside the loop!
{% endhighlight %}

It's because of this reason that you shouldn't write loops like that in Ruby. You don't want to bloat your memory out with all these unnecessary instance variables dangling around. 

Instead you should be using the **Enumerable** goodies provided to you. Use `.each` and limit the scope of the temporary variables to the `.each` function.

You can read more about this stuff in my pending Procs & Lambdas post.

{% highlight ruby %}
arr.each { |elem| puts elem }

     # now elem is not present outside each's block
elem # => NameError: undefined local variable or method `elem'
{% endhighlight %}

## Conclusion

When you're reading Ruby code you will inevitably find your own problems and raise your own questions, but things like the "0 is truthy" and when to use `self` caused me a great deal of head scratching. This has hopefully answered at least one preliminary code question from your reading. 
Have a play and next week i'll publish my notes from learning Lambda's and Procs. 



---

[1]: {% post_url 2015-01-25-java-to-ruby-series-announcement %} 
[2]: {% post_url 2014-12-27-response-to-booleans-in-method-sigs %}
[bearfarmer]: http://graphics8.nytimes.com/images/2011/07/08/arts/ZOO-span/ZOO-articleLarge.jpg


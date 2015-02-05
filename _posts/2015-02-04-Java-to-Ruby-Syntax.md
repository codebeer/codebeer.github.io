---
layout: post
title: "Java to Ruby - Syntactic Sugars"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "Ruby Codecation Post #2. A brief look at some syntax that makes Ruby so concise. Includes trouble with Booleans, Array manipulation and basic conventions."
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
`def print_animal_colours(animal, *colours)`

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

##Array Manipulation

You can easily add one array to another by joining the two existing arrays and simultaneously removing duplicates - all with a single operator!

`array | other_array`

You can also do the opposite, and simulate Set Intersection. This code returns a new array containing elements common only to both, with no duplicates.

`array & other_array`

### Array Shorthand

A very concise shortcut for creating arrays is using this neat suger-syntax. 

`story_words = %w{ This porridge is too hot }`

Here, each token is split by the whitespace and added to the array assigned to the `story_words` variable.

### Squashing Arrays 

You can append an item to the end of an array with the `<<` symbol. 
{% highlight ruby %}
#...
story_words << 'said'
story_words << 'Goldilocks'
puts story_words 
#=> ["This", "porridge", "is", "too", "hot", "said", "Goldilocks"]
{% endhighlight %}

You can even append one array to another. Also, this expression returns the array itself, so you can chain several appends together!

`array << array2`

However, now you have this situation!

![2D Array Diagram][2darray]

You have essentially created a 2 dimensional array. Which is possibly not what you intended. You wanted to join all items from one into the other, rather than add it as a third item

Use `flatten`.

{% highlight ruby %}
array1 = %w{ This porridge is too hot }
array2 = %w{ said Goldilocks.   }
array1 << array2
# => ["This", "porridge", "is" "too", "hot" , ["said", "Goldilocks"] ]
array1.flatten
puts array1
# =>  ["This", "porridge", "is" "too", "hot" , "said", "Goldilocks" ]
{% endhighlight %}

---

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


##Writing Boolean methods

Methods that return a boolean value should end with a `?`

ie: `empty?` would be written in Java as `isEmpty()`.

---

##Writing bang! methods 

An exclimation mark (`!`) or *bang* is used in Ruby to denote a **dangerous method**. This is a method that usually inflicts irreversable changes. 

For this reason, it is commonly mistaken to exclusively mean 'a method that alters variables *in place*'

### .sub! Example

Calling the `sub` method, like Java, will create a whole new string for the resulting substring.

`sub!` however will change the string **in place**. No new string is created. This could also be considered a permant change because the original value is *over-written*.

(another good example is chomp vs chomp! but there are loads of them)

Using a bang method like `chomp!` could also be used to ensure changes can persist outside of the concerned scope.(Like a 'pass by reference' almost.) 

---

## 1.9+ Hash Syntax

Check out a [recent post][2] that explains using symbols in Hashes and also details the v1.9 *'literal'* syntax.

In short, drop the symbol's prepended colon, and swap the `=>` for a `:`.

{% highlight ruby%}
old_way  = { :symbol_key => "value"} # bad (but common online)
new_way  = {  symbol_key:   "value"} # good :)
{% endhighlight %}

---

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

---

## Numerical Powers Syntax

I just wanted to mention this really quickly because I think it showcases the philosophical changes from Java. There is now math library here, or required Class reference - it's just part of the language - and it's super simple.

{% highlight ruby %}
num = 8
pow = num**2
puts pow
# => 64

# used on a collection
numbers = [2,3,4];
cubes = numbers.map{ |n| n**3 }

puts cubes
=> [8,27,64]
{% endhighlight %}  

---

##Conclusion

When you're reading Ruby code you will inevitably find your own problems and raise your own questions, but things like the "0 is truthy" caused me some head scratching. This has hopefully answered at least one granular preliminary code question from your reading. 

The [next post][post2] is more interesting and less granular. I'll take a look at OO syntax specifics and some hiccups I had with enumerables and hashes.

---

[1]: {% post_url 2015-01-25-java-to-ruby-series-announcement %} 
[2]: {% post_url 2014-12-27-response-to-booleans-in-method-sigs %}

[bearfarmer]: http://graphics8.nytimes.com/images/2011/07/08/arts/ZOO-span/ZOO-articleLarge.jpg
[2Darray]:  /images/posts/java2ruby/2Darray.png "2D Array"
[post2]: {% post_url 2015-02-05-Java-to-Ruby-Syntax-2 %} 
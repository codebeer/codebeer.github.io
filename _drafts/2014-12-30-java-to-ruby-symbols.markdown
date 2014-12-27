---
layout: post
title: "Java to Ruby - What are :Symbols?"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: ""
---

## Foreword

This belongs to a [Java to Ruby Series][1] of posts. Checkout that series link for a description of the series' intended audience and other posts available.

---

## What are :symbols ? 

Anything prefaced with a colon `:` is a symbol.  
This is a special tag used in Ruby.   
Basically, it's an immutable String.  

In that sense, very generally, you can mentally appreciate them like you would simple Strings. 

{% highlight ruby %}
:"Fuzzy Bear".to_s   # =>  "Fuzzy Bear"
 "Fuzzy Bear".intern # => :"Fuzzy Bear"
{% endhighlight %}

Even these unsetteling special characters are happilly accepted.

{% highlight ruby %}
danger_zone = "!@$"

 "Fuzzy Bear#{danger_zone}" # =>  "Fuzzy Bear!@$"
:"Fuzzy Bear#{danger_zone}" # => :"Fuzzy Bear!@$"
{% endhighlight %}

However, fundementally - they're **not** Strings.
Symbols are special objects in their own right - and you should appreciate that.

{% highlight ruby %}
shout = :talk.capitalize
puts shout
{% endhighlight %}
`./test.rb:2: undefined method 'capitalize' for :talk:Symbol (NoMethodError)`

--- 

## Common Misconceptions

Symbols are commonly confused or simplified for something like a C pointer.  
It's not, it's still a **value**. (technically, an *Object* - Ruby has no primitive types)

This confusion comes from their common use in Hashes (Hashes = Java Maps). Or, their 'shared' nature akin to static variables in Java.

From the [Ruby Docs][2] 

> The same Symbol object will be created for a given name or string for the duration of a program's execution, regardless of the context or meaning of that name. Thus if **Fred** is a constant in one context, a method in another, and a class in a third, the Symbol **:Fred** will be the same object in all three contexts.

### What?

Simply put - If you create a symbol, it will remain with you in all contexts, almost like a global. 

If you create a `:count` symbol for a Rabbit class, all rabbits will share the count - like a Java `static` field.  

---

## Save Time AND Space.

Okay, this is the fun bit. 

When creating a Map in Java, it is acceptable to use Strings as Keys or Values. 

{% highlight java %}
Map<String, String> Doctors = new HashMap<String, String>();
Doctors.put("D134", "Melbourne Medical Center");
{% endhighlight %}

Under the hood, the Map is happy because presumably the "D134" String Object key is unique and constant (As all Java Strings are **immutable**).  

However, in Ruby - Strings are **MUTABLE**.   

Changable values as a hash key? That sounds troublesome Matt. Good pickup reader, because you're right, it is.  

Using a String as a Hash key means Ruby needs to re-evaluate the string **every single time** it gets looked up, because it *could* have changed and therefor hash to a different spot in the map! 

So every 'Map' lookup using immutable keys would require...

1. Evaluate the String to get this key's contents.
2. Compute a Hash on that content.
3. Compare result against currently held key hashes in the map.
4. Hopefully find something.  


If you use a `:symbol` as a Hash key, it's implicit that it's immutable, it **can't** have changed, so Ruby can simply 

1. Immediately Hash the key's object-id.
2. Compare result against currently held key hashes in the map.  

This makes lookups much faster.   

---

## What About Space Saving? 

Consider the below code, making two Ruby hashes with a single key-value pair in each.

{% highlight ruby%}
first_hash  = { "string_key" => "value"}
second_hash = { "string_key" => "value"}
{% endhighlight %}

This has created **six** objects in memory, 2 Hash values, and 4 Strings.  

|----------|----------------------------|
| Hashes   | Strings                    |
|:-------- |:--------------------------:|
| `one`    | = { `two`      => `three`} |
| `four`   | = { `five`     => `six`  } |
|----------|----------------------------|

Now consider the alternative using `:symbols`

{% highlight ruby%}
first_hash  = { :symbol_key => "value"}
second_hash = { :symbol_key => "value"}
{% endhighlight %}

This created only **five** objects in memory, 1 symbol, 2 Hash values and 2 Strings. 

|----------|----------------------------|
| Hashes   | Strings                    |
|:-------- |:--------------------------:|
| `H1`     | = { `Sym`      => `Str1` } |
| `H2`     | = { `Sym`      => `Str2` } |
|----------|----------------------------|

The symbol key is shared between the two maps, it's still unique to each map individually so its totally legal friend.    

This is a trivial example but you can create your own hyperboles.   

---

## Fresh Syntax for Symbols in Hashes

If you're using a version of Ruby above 1.9 (and you should be) you can use the **literal syntax** for :symbols in a Hash.

Basically, you can drop the symbol's prepended colon, and swap the `=>` for a `:`.

{% highlight ruby%}
old_way  = { :symbol_key => "value"} # bad
new_way  = {  symbol_key:   "value"} # good :)
{% endhighlight %}

You can see this and many more Ruby style guidlines [here][3], and if you think this is absolutely too good to be true, [here][4] is the doc entry about it.

---

## Pro-Level

The Ruby Interpreter has a magical book called a 'Symbol Table' that holds onto all the :symbols you create - this table is protected from Garbage Collection in older versions of Ruby - so if you abused :symbols somehow maybe you could bloat your interpreter out.  (https://bugs.ruby-lang.org/issues/9634)

[1]: /
[2]: http://www.ruby-doc.org/core-2.1.4/Symbol.html
[3]: https://github.com/bbatsov/ruby-style-guide#hash-literals
[4]: http://ruby-doc.org/core-2.1.0/doc/syntax/literals_rdoc.html#label-Hashes
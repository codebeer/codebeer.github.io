---
layout: post
title: Making Jekyll blogging easier with Quiver
tags:
    - jekyll
    - quiver
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradley_curran"
excerpt: "Making static blogs that little bit better."
---

## Quiver: The Programmers Notebook

[Quiver](http://happenapps.com/#quiver) is an amazing tool for software developers. 

I've always felt like most note taking software I've used just haven't had the right set of features. On one side Evernote is too bloated. On the other side of the fence apps like nvALT feel too simple. 

As a developer I like having all my notes in plain text files that can be easily diffed and version controlled. I also want the ability to structure and orgnaise my notes into visual categories. It enables me to understand what I'm working on that much easier. 

That's where Quiver comes in. It takes the best parts from Evernote with it's notes and notebooks, together with storing notes as plain text from apps like nvALT and SimpleNote. 

It adds on top of these ideas with something integral: Structured notes. Or put another way - metadata associated with notes. 

## Structured note taking

The structure is quite nice. At a basic level, notes are broken up into cells. You can think of cells as paragraphs. A basic example is something like this: 

{% highlight json %}
{
  "title": "A Basic Note",
  "cells": [
    {
      "type": "text",
      "data": "Cell 1"
    },
    {
      "type": "text",
      "data": "Look, a second cell!"
    }
  ]
}
{% endhighlight %}

In this example, we have a note with a title of "A Basic Note" and two text cells. Adding metadata to something as simple as a note gives us a lot of power over a plain text file with unstructured text. 

Now we can: 

- Rearrange cells
- Filter cells that don't match a certain condition
- Map the structure from one format to another
- etc.

Quiver was built to make programming style note taking easy. Cells can be text, code, markdown or even latex. What this means is you can have a cell of code followed by a cell of text describing what the code does. 

{% highlight json %}
{
  "title": "Please don’t do this",
  "cells": [
    {
      "type": "markdown",
      "data": "How to ruin your day. "
    },
    {
      "type": "code",
      "language": "sh",
      "data": "$ sudo rm -rf /"
    }
  ]
}
{% endhighlight %}

Sound familiar? It's the same as technical blogging. You write text and code all in the same post. 

## Jekyll: Plain Text Blogging

[Jekyll](http://jekyllrb.com/) is great for hosting technical blogs. Posts are built in markdown and the site can be hosted for free on Github Pages. All in all it's a pretty good system. 

The downside is writing the posts themselves. Markdown can sometimes get in the way, especially when you need to combine code and text. 

## quiver2jekyll: Getting the best of both worlds

This is where the beauty of structured plain text files comes in. I wrote a script called [quiver2jekyll](https://github.com/bradley-curran/quiver2jekyll) to convert Quiver notes into Jekyll posts. This post was put together in Quiver, exported using quiver2jekyll and added to Jekyll. 

The script takes the metadata.json and content.json from an exported Quiver note and constructs a markdown file with language specific code highlighting applied. The YAML front matter is dynamically generated using the Quiver note's title and tags. 

Given the "Please don't do this" example above, quiver2jekyll will generate the markdown file 2015-01-04-please-don’t-do-this.markdown: 

<pre>
---
layout: post
title: Please don’t do this
tags:
    - command-line
    - linux
---

How to ruin your day. 

{% raw %}
{% highlight sh %}
$ sudo rm -rf /
{% endhighlight %}
{% endraw %}
</pre>

Once the file is generated all you need to do is move the file into your `_posts` folder in your Jekyll blog. 

## Installing quiver2jekyll

I've setup a brew tap to make it easy to install quiver2jekyll. [Install homebrew](http://brew.sh/) if you haven't already. It's awesome. 

{% highlight sh %}
$ brew tap bradley-curran/tap
$ brew install quiver2jekyll
{% endhighlight %}

## Using quiver2jekyll

- Export your Quiver note by selecting it in Quiver and selecting File > Export Note...
- Save it to somewhere memorable, like the Desktop
- Open Terminal and run `$ cd ~/Desktop`
- Run `$ quiver2jekyll <exportedfile.qvnote>`

## Future Features

The next thing I'd really like to add is exporting images that are stored in the Quiver note resources/ folder. I haven't looked into the differences between Quiver's image location and Jekyll's, though I imagine the script will need to know the image directory that Jekyll uses. 

Another good feature would be to automatically put the generated markdown post into the Jekyll `_posts` folder. That would reduce post writing turnaround time significantly. 

Once the script knows the location of the Jekyll site it can be a lot more powerful. Reading the YAML configuration files opens it up to a whole lot more dynamic functionality. 


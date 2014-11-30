---
layout: post
title: "Should booleans be used in method signatures?"
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradley_curran"
excerpt: "Well, it all depends. "
---
Early on when I was learning how to write software I'd often add a boolean to extend functionality of a method. Most developers do this at some stage and eventually get the feeling that something is wrong. This post tries to explain what the problem is and how to write code that's easier to maintain. 

Below is an example piece of code that draws a list of views to the screen: 

{% highlight java %}
public void drawViews() {
    for (Item item : this.list) {
        drawView(item);
    }
}
{% endhighlight %}

The users have spoken and now they want the option to have a dark theme. What's a good way of implementing this? 

The first thought that comes to mind is modifying the existing drawViews() method to draw a light or dark theme based on a boolean. That would look something like this: 

{% highlight java %}
public void drawViews(boolean darkTheme) {
    for (Item item : this.list) {
        if (darkTheme) {
            drawDarkView(item);
        } else {
            drawView(item);
        }
    }
}
{% endhighlight %}

This seems innocent enough. If the boolean is true we draw a dark theme, otherwise we draw the old light theme. 

Let's look at it from the calling code. 

{% highlight java %}
// what does that boolean parameter mean?
// does it mean "don't draw the views"?
// I'm so confused. :( FML
drawViews(false);
{% endhighlight %}

If another developer is calling your method it's not clear what the boolean parameter does. 

Without looking into the method implementation it looks like at first glance that the method only draws the views if the boolean passed in is true. But that's not what the code does at all! 

### How do we fix this?

So why did something that was so simple to implement make the code base harder to use? 

There's some guidelines about Booleans in Method Signatures that you should know. 

**BiMS Guideline #1: Use booleans in method signatures for simple setters**

This is an easy guideline to follow. Sometimes you want to set the internal state of an object with a boolean. A good example of this is visibility of some UI component: 

{% highlight java %}
public void setVisible(boolean visible) {
    this.visible = visible;
}
{% endhighlight %}

**BiMS Guideline #2: Avoid booleans in method signatures anywhere else**

If there's a boolean in a method signature for any other reason than #1, you're most likely making your API more complicated than it needs to be. 

That's it. Pretty simple huh? So what's a better way to implement the dark theme feature from before? 

#### Replace conditional logic with two separate methods

Given a generalised version of the code above:

{% highlight java %}
doTheThing(boolean codePathA) {
    // common functionality

    if (codePathA) {
        // do A
    } else {
        // do B
    }
}
{% endhighlight %}

Extract the common functionality into its own method and break code path A and code path B into their own methods. 

{% highlight java %}
doTheThingA() {
    commonFunctionality();
    // do A
}

doTheThingB() {
    commonFunctionality();
    // do B
}

commonFunctionality() {
    // common functionality
}
{% endhighlight %}

#### Replace conditional logic with the Strategy Pattern

Alternatively we can be really tricky and extract the functionality of drawing a view into a separate object. This is known as the [Strategy Pattern](https://en.wikipedia.org/wiki/Strategy_pattern). 

We define an interface like so: 

{% highlight java %}
public interface ViewDrawer { // I know, it's a horrible name :)
    public void drawView(Item item);
}
{% endhighlight %}

And we'd have two implementations, one for each theme: 

{% highlight java %}
public class LightViewDrawer implements ViewDrawer { ... }
public class DarkViewDrawer implements ViewDrawer { ... }
{% endhighlight %}

Then your drawViews() method would stay almost exactly the same as the original: 

{% highlight java %}
public void drawViews() {
    for (Item item : this.list) {
        this.viewDrawer.drawView(item);
    }
}
{% endhighlight %}

The difference is that we've delegated the drawing of a view to a separate object. One that can be assigned at run time rather than compile time. 

This way we can easily add more themes as the users request them. Our code is more extensible and easier to maintain. 

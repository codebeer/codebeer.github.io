---
layout: post
title: "Why you should give LayoutInflater a root"
author: "Brad Curran"
---
We've discussed the sometimes mysterious [LayoutInflater]({% post_url 2014-10-26-layoutinflater-basics %}) before, but I wanted to talk about one feature in particular that I have only found out recently, even though I've been developing Android applications for a few years now. 

LayoutInflater has two methods you'll use the most: 

{% highlight java %}
View inflate(int resource, ViewGroup root)
View inflate(int resource, ViewGroup root, boolean attachToRoot)
{% endhighlight %}

I would often supply null as the root for the layout inflater call: 

{% highlight java %}
View view = inflater.inflate(R.layout.someview, null);
{% endhighlight %}

While this is valid, it turns out that by providing null as the root for the second parameter I'm actually making a big mistake. 

Have you ever wondered why when you inflate a layout for an Adapter or a Fragment that all the margins aren't applied, even though they're specified in your XML? It's frustrating to set it all up in the editor and not see it reflected in the app. 

There's two types of XML view attribute names. One type starts with `android:layout_` and ones that don't. The `layout_*` values are evaluated according to the parent View. The others are evaluated on the View itself. 

So if a View isn't supplied with a root when it's inflated then all of the `layout_*` values will be thrown away! That's definitely not what we want. 

## Using LayoutInflater the right way

The two main times when I use LayoutInflater is in Fragments and Adapters. Fortunately in both of these cases we're supplied with the root View when we want to inflate a layout. 

### Adapters

{% highlight java %}
@Override
public View getView (int position, View convertView, ViewGroup parent) {
    // simple example, normally you use view holder etc.
    return LayoutInflater.from(context).inflate(R.layout.row, parent, false);
}
{% endhighlight %}

### Fragments

{% highlight java %}
public View onCreateView (LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    return LayoutInflater.from(context).inflate(R.layout.fragment, parent, false);
}
{% endhighlight %}

By supplying a root to the layout inflation you get the margins you asked for. 

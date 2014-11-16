---
layout: post
title: "LayoutInflater Basics"
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradleycurran"
excerpt: "Introduction to Layout Basics. Hold onto those hats."

---
If you're new to Android Development you've probably seen this guy called [LayoutInflater](http://developer.android.com/reference/android/view/LayoutInflater.html) and you're wondering what it does. Fortunately it's pretty easy to explain. 

## The hell is a LayoutInflator anyway?

Your first Android app would have had a bit of code like this: 

{% highlight java %}
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
{% endhighlight %}

setContentView is the method you call to set a layout xml file in res/layout as the View for an Activity. 

Internally Android uses LayoutInflater to convert your activity_main.xml into a set of Views to show on the screen. 

Simply put, LayoutInflater is a class that converts an layout xml file into a View hierarchy. 

## Okay cool, so when do I need to use LayoutInflater?  

Whenever you need to create a View from xml! 

The places where you'll typically need to create a View is when you're making a custom [Adapter](http://developer.android.com/reference/android/widget/Adapter.html) for a ListView. Another time you'll create a View is when you create a [Fragment](http://developer.android.com/guide/components/fragments.html). 

## That seems simple enough. How do I use it? 

### For Fragments

{% highlight java %}
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_main, container, false);
}
{% endhighlight %}

### For Adapters

{% highlight java %}
@Override
public View getView(int position, View convertView, ViewGroup parent) {
    View view = convertView;

    if (convertView == null) {
        LayoutInflater inflater = LayoutInflater.from(parent.getContext());
        view = inflater.inflate(R.layout.row, parent, false);
    }

    return view;
}
{% endhighlight %}

### For Anything Else

My favourite way to get the LayoutInflater is by using the static factory method on LayoutInflater. 

{% highlight java %}
LayoutInflater inflater = LayoutInflater.from(context);
{% endhighlight %}

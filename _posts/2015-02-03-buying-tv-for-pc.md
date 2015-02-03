---
layout: post
title: "Buying a TV for your PC"
author: 
    name: "Matt Witherow"
    image: "matt.png"
    twitter: "MattJWitherow"
excerpt: "Avoid stuttering, tearing, lag and ghosting. You can entirely ruin your PC gaming if you're not careful. This accompanies my 'I Built a SteamBox' post."
---

This accompanies my post '[I Built a SteamBox][steambox]'.   
I broke the TV specifics into a separate post rather than gloss over it - because your choice of TV can completely ruin the output of your expensive PC hardware if you're not careful. 

Here is just a few basic concepts to be aware of when buying.

---

### My TV, Sony Bravia W802A

The Sony Bravia 802 comes in a 42" or 55" size.
It is also identical to [this][tv805] 47" model, that is better documented.

The trailing letter is simply an area code (**A** for 'Austral-Asia' i think), in Europe it may be an **E**, rest assured its an identical model. 

There are **two main things** you need consider when talking about TVs specifically for gaming.

---

### Input Delay 

This refers to the delay from the moment a display receives a signal from a game controller to the results rendering on the screen and ultimately received by your eyes. 

TVs are getting smarter and smarter with firmware and software, but ultimately this creates more layers for signals to traverse and actually contributes to higher input lag. When you activate the *game mode* of a TV, it is essentially **turning off** as much unnecessary software as possible to open up the pipes.

There are two common ways that testers measure this delay.  

+ Elaborate high speed camera setups
+ [Leo Bodnar][lbtest] test hardware

Each method measures slightly different things, so make sure you know which one was used when comparing articles online. The Leo Bodnar test will be far more accurate, but will also yield a larger number. 

#### Target Times

As a rule, find a TV with an input delay **less than 16ms (photo)** or **25ms (Leo Bodnar)**.

My Sony Bravia  is **6ms (photo)** and **17ms (LB)**. 

You can read an old general review of the TV from CNET [here][tvcnet].

Find some TVs you like in your local stores, then check online at websites like [this][lagtest] to check they meet these targets.

---

### Refresh Rates

The second factor to consider is the refresh rate of your TV.   

![screen tear](http://upload.wikimedia.org/wikipedia/commons/0/03/Tearing_%28simulated%29.jpg "Screen Tearing Example By Vanessaezekowitz")  

This image shows an example of my pet hate, **[Screen Tearing][screentearing]**.

Tearing is caused because your TV refreshes the image at a fixed rate, but your GPU outputs at a changing rate.   
When the frame supplied by your graphics card changes during a refresh cycle on the TV, you get several images on screen at once, hence tearing.

The widely used VSync Feature fixes that problem by capping your PC's FPS at the refresh rate of your monitor. However, if your framerate drops below the rate of the monitor, you get a different stuttering effect because the GPU is sending the same frame twice while it's waiting for the next one to be rendered.

Bottom line, buy a TV with a high refresh rate.

When I'm not playing games on the TV I'm watching football and the higher rate makes all the difference. It's important to understand however that the rates claimed by the TV manufacturers *aren't exactly real*, they are achieved through a process called **interpolation**.

[Traditional Interpolation][tradimpol] is about *guessing* where pixels should go and what they should be to *fill in* the spaces. This is used when you scale an image.

When talking about TVs, we're talking about [Motion Interpolation][motionimpol]. It's exactly the same principle. 
Say we have a 50Hz TV, this refreshes 50 times a second. So, it can show 50 unique frames a second.

If we numbered these frames we would have something like...

F1, F2, F3, F4, F5, F6, F7........F50.

Motion Interpolation is about *guessing* the frames **in between** each frame, just like missing pixels in traditional interpolation. If you consider a man running playing football we could split the difference between F1 and F2, we could imagine this new frame (F1.5) would show the man's running legs halfway between their positions shown in F1 and F2. 

If you do this for all frames, you would have the following.

F1, **F1.5**, F2, **F2.5**, F3, **F3.5** .....F50.

You have now **doubled** your frames, and if we still show them all within the same second, we have achieved an 100Hz TV.

It's no surprise that Motion Interpolation helps your TV show *fast contrast changes*, such as football players quickly moving over green grass. Some TVs will have better algorithms for splitting or *guessing* the frames than others.  

You may notice on cheaper TVs or on TVs with lower refresh rates that when watching sports you see a 'ghosting' or white glow around players. This is an example of your TV not entirely guessing the correct intermediate frame. This may mean the sportsman is painted moving in a direction that he ultimately did not take. This can be perceived by your eyes as glowing edges around the contrasting artifacts (the player and the grass behind him).

**Important for Aussies:** You may notice American TVs are sold as 60Hz or 120Hz, but Australian TVs are sold as 50Hz and 100Hz. 

To get the most out of your PC + TV gaming setup, when using VSync, you should reduce the capped frame output to 50HZ (from the American set 60Hz). You can do this through the software that came with the graphics card. This will make your PCs output a *common factor* of the TVs refresh rate (eg: 100 is divisible 50, but not 60). 

This will allow you to crank up your picture quality a little bit (because the card doesn't have to produce as many frames) and now let the **TV hardware** up the frames using its own interpolation processing.

---

## A Word on Sony

After a few weeks of asking around and online research, I found that on average Sony seemed to be recommended the most for gaming TVs. When I talked to another engineer about this, who had experience with electrical manufacturing and firmware - he pointed out that it was in Sony's best interest to provide the best *game mode* over other TV brands, because Sony themselves make the gaming consoles. 

Further online reading suggested that Sony's *game mode* seemed to cull the most in-memory software in their smart TVs when activated - allowing a short circuit path for input signals. They had a vested interested to make this mode effective for their own PlayStation Consoles. 

Sony could be a safe bet. I know I'm happy with mine.

---

## Conclusion

As always, technology is constantly outdated - so there are no doubt better choices than my exact TV in 2015. 
Before you buy, try to find the Input Delay measurements online and keep them below the targets. 
Fork out for higher refresh rates, and keep them divisible with your inputs.  
Vsync is a pretty solid solution, but to keep it from stuttering you may need to reduce the graphics quality of games as your hardware ages. 

Take note of the port interfaces, at this enthusiasts level - HDMI transport is really at it's limit. You can only pump so much down the line from your beastly PC.
Check what the next thing is. 


[steambox]: {% post_url 2015-02-03-I-Built-a-Steam-Box %}
[lagtest]: http://www.hdtvtest.co.uk/news/input-lag
[tvcnet]: http://www.cnet.com/products/sony-kdl-55w802a/
[lbtest]: http://www.leobodnar.com/shop/index.php?main_page=product_info&products_id=212
[tv805]: http://www.hdtvtest.co.uk/news/sony-kdl47w805a-201307043162.htm
[screentearing]: http://en.wikipedia.org/wiki/Screen_tearing
[tradimpol]: http://www.cambridgeincolour.com/tutorials/image-interpolation.htm
[motionimpol]: http://en.wikipedia.org/wiki/Motion_interpolation




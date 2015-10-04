---
layout: post
title: How to automatically format and optimize imports in Android Studio
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradley_curran"
excerpt: "Trying to force Android Studio to be Eclipse. One macro at a damn time. "
tags:
    - android studio
    - intellij
---

There's nothing worse than working with a file that has no formatting applied to it and a million unused imports.  

Coming from Eclipse I enjoyed hitting Cmd + S and automatically having my code clean all of this up for me. So I was surprised that Android Studio doesn't have this functionality out of the box. 

Fortunately IntelliJ/Android Studio gives you the ability to reproduce this using macros. 

The walkthrough below assumes you use OSX. If you use Windows or Linux it should be the same just with different shortcut keys. 

## Creating the macro ##

1. Open a source code file
1. Hit Ctrl + Alt + O. This optimizes imports. 
1.* If a dialog shows up check the box "Do not show this message again" and click Run.
1. Go to Edit > Macros > Start Macro Recording
1. Hit Ctrl + Alt + O
1. Hit Cmd + Alt + L. This formats your code. 
1. Hit Cmd + S
1. Go to Edit > Macros > Stop Macro Recording
1. Save the macro under a useful name
1.* I used "Optimize imports, format code and save"

## Assigning the macro to a keyboard shortcut ##

1. Open Preferences
1. Search in the left menu bar for Keymap
1. In the right hand pane, click in the search bar and type the name of your saved macro
1. Double click on your item. There might be two, it doesn't matter which one you click on. 
1. Click Add Keyboard Shortcut
1. Set your keyboard shortcut to Cmd + S
1.* If you don't want to override your normal save you can assign it to something like Cmd + Alt + Shift + S
1. Confirm in the dialog that you want to override Cmd + S to be your new macro

Congratualtions! Every time you hit Cmd + S you'll automatically format and optimize the imports in your code. 


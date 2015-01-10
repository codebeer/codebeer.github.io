---
layout: post
title: Creating your own Homebrew Tap and Formula
tags:
    - homebrew
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradley_curran"
excerpt: "A million lines for you. Two lines for everyone else. "
---

Distributing command line apps and scripts that you've written on OSX is a lot more complicated than I thought. 

Sure you can ask your users to go the manual route. But tell me, which are you more likely to do as a user? 

### Manual

- Download my quiver2jekyll script: https://raw.githubusercontent.com/bradley-curran/quiver2jekyll/9ad8ffe4511fe5ee7f08ee25eeb8bf24962deab1/quiver2jekyll
- `$ cd ~/Downloads`
- `$ chmod u+x quiver2jekyll`
- `$ sudo mv quiver2jekyll /usr/local/bin`

### Or the homebrew (v0.9 and up) way

{% highlight sh %}
$ brew tap bradley-curran/tap
$ brew install quiver2jekyll
{% endhighlight %}

Here's the benefits that the homebrew way offers:

- Simpler syntax on the terminal
- Less user resistance to installing your app
- Your command line app is version controlled and can now be upgraded using `$ brew upgrade`

If you have an app or script you want to distribute to the OSX community, homebrew is a great way to do it. 

Semi unfortunately, the brew community won't just add any script/app/whatever to brew. If your script is too niche (like most of the things I write), [it probably won't be accepted into the default homebrew tap](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Acceptable-Formulae.md#niche-or-self-submitted-stuff). 

Fortunately homebrew added the `brew tap` feature in 0.9. `brew tap` allows users to add custom formulae to brew. 

Before we go any further, let's have a terminology lesson. :)

- Keg: An installed script/app/etc.
- Cellar: A list of all the installed kegs
- formula: A ruby script that defines how to download, compile and install a keg
- tap: A list of formulas stored in a Github repository

So essentially what we need to do is create a tap (Github repository) that contains a formula (install script) that adds a Keg (downloaded script) to our Cellar (list of locally installed scripts). 

Go go software metaphors! 

## Overview

We're going to create a groundbreaking script that echos `Formal Friday Club rocks!` to the terminal. 

This script will be downloaded and installed by your formula in your custom tap using the commands:

{% highlight sh %}
$ brew tap github-username/tap
$ brew install formalfridayclub
$ formalfriday
Formal Friday Club rocks!
{% endhighlight %}

## Step by Step Instructions

These instructions are based on homebrew's [Formula Cookbook](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Formula-Cookbook.md) together with my own insights into how homebrew and taps work. The Formula Cookbook is the authoritative method to creating your own formula, but it's also hella difficult to understand IMO. :)

### Prerequisites and Assumptions

- You're running OSX
- You have installed [homebrew](brew.sh) and it exists in `/usr/local/`
- You have a Github account
- You're competent with git

### Create the script and its repository

What we're doing in this step is creating the script itself and putting it in a Github repository. We do this so that we can create archives of versions we want to release to the public. 

Go to <https://github.com/new> and call the repository formalfridayclub. Make sure that "Initialize this repository with a README" is checked. Your screen should look like this: 

![Repository setup](/images/posts/9D5D24B5-FF87-4900-82FD-2BE04FD7E5E0.png)

Open your Terminal and run the following commands 

{% highlight sh %}
$ git clone git@github.com:username/formalfridayclub.git
$ cd formalfridayclub
$ echo '#!/bin/sh
> echo Formal Friday Club rocks!' > formalfriday
$ chmod u+x formalfriday
$ ./formalfriday
Formal Friday Club rocks!
{% endhighlight %}

If everything worked, you should see "Formal Friday Club rocks!" as the last line in your terminal output. 

Now that you've created that groundbreaking script, let's push the changes to Github. 

{% highlight sh %}
$ git add .
$ git commit -m "Groundbreaking script"
$ git push origin master
{% endhighlight %}

### Create the archive

Well, I don't know about you but I think that this script is definitely worthy of 1.0.0 status. Let's tag the repository so that Github creates an archive for us. Once we get the link to the archive we can create our homebrew formula. 

Bring up the terminal again and let's tag the repo with 1.0.0

{% highlight sh %}
$ git tag 1.0.0
$ git push --tags
{% endhighlight %}

When we push a tag with this format, we create a release on Github. You can access the releases by going to your repository homepage and selecting Releases in the repository toolbar or by going to https://github.com/username/formalfridayclub/releases. 
You should see something like this: 

![Github Releases](/images/posts/76215E8F-0A79-4514-BC0D-B6F331931E96.png)

Awesome! By tagging the repo with 1.0.0, Github has created an archive complete with download links for the .zip and .tar.gz. 

### Create the formula

Now that we have the archive link to the .tar.gz file, we can create our homebrew formula. 

Copy the link address of the tar.gz file. Mine looks like <https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz>

Now back at your terminal, type the following command, substituting my link for yours: 

{% highlight sh %}
$ brew create https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz
{% endhighlight %}

This will create a generated formula file at the location `/usr/local/Library/Formula/formalfridayclub.rb`. This file is the set of instructions to download and install the script. It will look something like this: 

{% highlight ruby %}
# Documentation: https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Formula-Cookbook.md
#                /usr/local/Library/Contributions/example-formula.rb
# PLEASE REMOVE ALL GENERATED COMMENTS BEFORE SUBMITTING YOUR PULL REQUEST!

class Formalfridayclub < Formula
  homepage ""
  url "https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz"
  sha1 ""

  # depends_on "cmake" => :build
  depends_on :x11 # if your formula requires any X11/XQuartz components

  def install
    # ENV.deparallelize  # if your formula fails when building in parallel

    # Remove unrecognized options if warned by configure
    system "./configure", "--disable-debug",
                          "--disable-dependency-tracking",
                          "--disable-silent-rules",
                          "--prefix=#{prefix}"
    # system "cmake", ".", *std_cmake_args
    system "make", "install" # if this fails, try separate make/make install steps
  end

  test do
    # `test do` will create, run in and delete a temporary directory.
    #
    # This test will fail and we won't accept that! It's enough to just replace
    # "false" with the main program this formula installs, but it'd be nice if you
    # were more thorough. Run the test with `brew test formalfridayclub`. Options passed
    # to `brew install` such as `--HEAD` also need to be provided to `brew test`.
    #
    # The installed folder is not in the path, so use the entire path to any
    # executables being tested: `system "#{bin}/program", "do", "something"`.
    system "false"
  end
end
{% endhighlight %}

Let's do as the header comments suggest and remove all the generated comments. While we're at it, let's remove any unnecessary example calls as well. 

{% highlight ruby %}
class Formalfridayclub < Formula
  homepage ""
  url "https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz"
  sha1 ""

  def install
  end
end
{% endhighlight %}

The homepage value is used by `brew home <formula>`. In real homebrew formulae we'd set this to an actual homepage, but for ours we can just put the location of the Github repo. 

The url was added by `brew create` call. This doesn't need to change. 

Before we set sha1, let's give the script a shot. We run the formula by calling `brew install <formula>`. 

{% highlight sh %}
$ brew install formalfridayclub
==> Downloading https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz
######################################################################## 100.0%
Warning: Cannot verify integrity of formalfridayclub-1.0.0.tar.gz
A checksum was not provided for this resource
For your reference the SHA1 is: 0a323c749241b0dcb313b3d4cc9411d8b34bbf75
==> Patching
🍺  /usr/local/Cellar/formalfridayclub/1.0.0: 2 files, 8.0K, built in 2 seconds
{% endhighlight %}

Well, that was a freebie. When we called `brew install formalfridayclub` it downloaded the archive specified in `url` in the formula and placed it in our Cellar, but didn't do much more other than that. The cache of the downloaded archive is stored in `/Library/Caches/Homebrew`, which means we don't have to redownload it the next time we run `brew install formalfridayclub`. 

Let's copy that SHA1 from the output and put it into our formula. 

{% highlight ruby %}
class Formalfridayclub < Formula
  homepage "https://github.com/bradley-curran/formalfridayclub"
  url "https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz"
  sha1 "0a323c749241b0dcb313b3d4cc9411d8b34bbf75"

  def install
  end
end
{% endhighlight %}

Last but not least we need to add our script to the formalfridayclub keg. We do this by adding specific commands to the install method in our formula. 

When the install method is run in your formula the current directory is the extracted tarball. You can access various parts of the keg and homebrew itself by [using these variables](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Formula-Cookbook.md#just-copying-some-files). 

What we need to do is copy the formalfriday script into the keg's bin/ directory. We can use the bin variable to do this: 

{% highlight ruby %}
class Formalfridayclub < Formula
  homepage "https://github.com/bradley-curran/formalfridayclub"
  url "https://github.com/bradley-curran/formalfridayclub/archive/1.0.0.tar.gz"
  sha1 "0a323c749241b0dcb313b3d4cc9411d8b34bbf75"

  def install
  	bin.install "formalfriday"
  end
end
{% endhighlight %}

Run the following command to reinstall formalfriday: 

{% highlight sh %}
$ brew reinstall formalfridayclub
{% endhighlight %}

Now with any luck homebrew has added your formalfriday script to the formalfridayclub keg in the cellar. This means that if you type: 

{% highlight sh %}
$ which formalfriday
/usr/local/bin/formalfriday
$ formalfriday
Formal Friday Club rocks!
{% endhighlight %}

Success! Homebrew has pulled the version 1.0.0 of the formalfriday script from the internet and put it on your path. 

What can't the internet do? 

### Create your own Tap

This is all well and good, but this doesn't mean much unless other people can also use your formula. To do this we need to create our own tap as homebrew probably won't add a formula that echos some text to the screen. 

Making a tap is easy. All we need to do is create a new repo on Github with a valid name. In this example I'm going to call it bradley-curran/homebrew-tap, but as long as your repo starts with "homebrew-" you should be okay. 

Clone the repo and move `/usr/local/Library/Formula/formalfridayclub.rb` into the root of your homebrew-tap repo. A good example to take a look at is [homebrew/homebrew-games](https://github.com/Homebrew/homebrew-games). Commit and push the file to Github. 

Uninstall your existing formalfridayclub keg. 

{% highlight sh %}
$ brew uninstall formalfridayclub
{% endhighlight %}

Now the real fun begins. Add your custom tap then install the formalfridayclub keg. 

{% highlight sh %}
$ brew tap username/tap # this refers to your repo, with the "homebrew-" removed
$ brew install formalfridayclub
{% endhighlight %}

Congratulations! Those two commands you just typed are the same two commands anyone else with homebrew can type in order to install your script. 

The formula is added to their brew when calling `brew tap bradley-curran/tap` and is installed by calling `brew install formalfridayclub`. 

While this is a convoluted example of how to get some text to be displayed on someone else's screen it does show that you can write software that you can distribute to others easily. 


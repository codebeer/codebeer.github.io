---
layout: post
title: "How to encrypt files on OS X by the command line"
author: 
    name: "Brad Curran"
    image: "brad.png"
    twitter: "bradleycurran"
---
OS X has a great encryption utility called [FileVault](http://support.apple.com/kb/ht4790). While simple, out of the way full disk encryption is great, it doesn't help so much when you want to encrypt a single file to store securely. 

Turns out [OpenSSL](https://www.openssl.org/) can help us out here. OpenSSL is an open source toolkit that has an implementation of Secure Sockets Layer v2/v3. It also has an entire cryptography library. We can utilise the crypto library to encrypt and decrypt individual files. 

OS X has openssl installed on it by default, which makes things easier for us. We can encrypt and decrypt files by using openssl on the command line. 

## Encrypting a File

Open Terminal and navigate to the directory of the file you want to encrypt. For this example let's say the file is topSecretData.txt. 

{% highlight bash %}
~ $ openssl enc -aes-256-cbc -salt -in topSecretData.txt -out data.enc
{% endhighlight %}

`enc` means "I want to encode something with a cipher"

`-aes-256-cbc` is the cipher algorithm you want to use. AES256 CBC is a good choice. Unless you know what you're doing, stick with this algorithm. 

`-salt` this is poorly named. What this actually means is that it's adding an Initialisation Vector to the encryption. Not adding this option makes the encryption much weaker. Make sure to have this option. 

`-in topSecretData.txt` this is the file you want to encrypt. 

`-out data.enc` this will be the name of your encrypted file. 

## Decrypting a file

With Terminal open and at the directory where your encrypted file is, type the following: 

{% highlight bash %}
openssl enc -d -aes-256-cbc -salt -in data.enc > topSecretData.txt
{% endhighlight %}

`enc -d` means "I want to decode something with a cipher". Notice that this has a `-d`, this is because the decrypting a file is a different process to encrypting it. 

`-aes-256-cbc` make sure to use the algorithm for decryption as you did for encryption. 

`-salt` if you used `-salt` to encrypt, make sure to use it decrypting as well. 

`-in data.enc` this is the name of your encrypted file. 

`> topSecretData.txt` decryption has a slightly different syntax to encryption. The decryption gets output to the output stream rather than us specifying a file to output to using `-out`. 

## Optional Funtimes

If you want your encrypted file to be in text rather than binary you can use the `-a` flag just after the cipher algorithm. 

So in our above encryption case: 

{% highlight bash %}
~ $ openssl enc -aes-256-cbc -a -salt -in topSecretData.txt -out data.enc
{% endhighlight %}

Just like all the other flags, make sure to use the `-a` flag again when decrypting. 

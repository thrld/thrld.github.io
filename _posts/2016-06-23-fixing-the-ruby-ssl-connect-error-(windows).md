---
title: "Fixing the Ruby SSL connect error (Windows)"
date: 2016-06-23
section: blog
output: html_document
layout: post
---

A short post in case some of you are experiencing similar problems with **Ruby** on Windows. When installing a gem (e.g. 'jekyll') over cmd via ```gem install jekyll```, I got an error message saying that ''certificate verify failed''. This is annoying, especially since it was hard to find a solution. After some research, I found a working solution by combining suggestions from two different blog posts ([#1](https://gist.github.com/luislavena/f064211759ee0f806c88), [#2](https://superdevresources.com/ssl-error-ruby-gems-windows/)).

**4 simple steps:**

**1.** Locate your Ruby directory: Open the command prompt and type 
{% highlight text %}
gem which rubygems
{% endhighlight %}
In my case, this returned ```C:/Ruby200-x64/lib/ruby/2.0.0/rubygems.rb```, but might also be something else, e.g., ```C:/Ruby21/lib/ruby/2.1.0/rubygems.rb```.

**2.** Open the directory by typing 
{% highlight text %}
start C:/Ruby200-x64/lib/ruby/2.0.0
{% endhighlight %}
(or whatever your file path is) into your command prompt. An explorer window will open. Now locate the ```ssl_certs``` directory (try ```./rubygems/ssl_certs```). 

**3.** Download a [new certificate](https://curl.haxx.se/ca/cacert.pem). Make sure the file has a **.pem** extension (Chrome likes to save stuff as .txt file -- in this case, just change it back manually). Save it in the ```ssl_certs``` directory we located in 2.

**4.** Retry ```gem install jekyll``` (or whatever it is you want to install).

**Good luck, hope it works for you too!** 
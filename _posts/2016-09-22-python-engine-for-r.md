---
title: "Python Engine for R"
date: 2016-09-22
section: blog
output: html_document
layout: post
---

R is making progress when it comes to improving the interoperability between Python and R. Note that what you are looking at here is a .Rmd file. Using ```` ```{python} ```` instead of ```` ```{R} ```` activates the Python engine. This means that R evaluates Pythong commands now.





{% highlight python %}
import sys
print(sys.version)
{% endhighlight %}




{% highlight text %}
## 3.4.3 |Anaconda 2.3.0 (64-bit)| (default, Mar  6 2015, 12:06:10) [MSC v.1600 64 bit (AMD64)]
{% endhighlight %}


{% highlight python %}
x = 'hello, python world!'
print(x.split(' '))
{% endhighlight %}




{% highlight text %}
## ['hello,', 'python', 'world!']
{% endhighlight %}

PS: Wes McKinney and Hadley Wickham even seem to be working on a common data format called *Feather*. More on this exciting new development can be found [here](https://blog.rstudio.org/2016/03/29/feather/). 

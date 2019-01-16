---
title: "Syntax Highlighting on Github Pages"
date: 2018-07-04
section: blog
output: html_document
layout: post
---

I stumbled across some blog posts that dealt with syntax highlighting on Github Pages last week. A number of posts favoured **Pygments**. However, when I had everything up and running and pushed the changes up to Github, I got an email from them with an error message saying that ''the pygments highlighter [...] is currently unsupported on Github Pages'' and that I should use **Rouge** instead (also see [this blog post from Github](https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0)). 

I did a little bit of research and it turns out that Rouge themes are 100% Pygments compatible. That is a great plus since I had already used Pygments to create a css file for syntax highlighting. 

**Here is a short rundown of what I did.**

While Rouge is written in Ruby, Pygments is written Python. This means that once you have installed the **pygmentize** script, you can use Pygments from the shell in a number of ways ([this article](http://pygments.org/docs/cmdline/) gives more details). I generated a basic css file by typing the following into my cmd: 

{% highlight text %}
pygmentize -f html -S default > path/to/css/highlight.css
{% endhighlight %}

This will create a css file and save it at the specified location. Then simply import it to your main css file that you are using to style your site by adding the following line (recommendation: [at the very top](https://www.w3.org/TR/CSS2/cascade.html#at-import)): 

{% highlight text %}
@import url("highlights.css");
{% endhighlight %}

Note that this assumes that you saved the generated highlights.css to the location where your main file lives. I previewed the results and made some minor changes to the css file (make sure to have a look at [this blog post](https://monicagranbois.com/blog/webdev/formatting-code-with-pygments-and-jekyll/) for  more advice on editing the file).

**Two more things.** 

**1.** You need to use liquid tags for the highlighting to work, that is:

{% highlight text %}
{% raw %}{% highlight python %}{% endraw %}
yourCodeHere
{% raw %}{% endhighlight %}{% endraw %}
{% endhighlight %}

Of course, ```python``` is not the only choice you have here -- support is (almost) endless, but of varying quality, depending on which language you are looking for.

Here is a simple example in Python:

{% highlight text %}
{% raw %}{% highlight python %}{% endraw %}
print(x.split(' '))
{% raw %}{% endhighlight %}{% endraw %}
{% endhighlight %}

produces

{% highlight python %}
x = 'hello, python world!'
print(x.split(' '))
{% endhighlight %}

which would yield the result:

{% highlight text %}
## ['hello,', 'python', 'world!']
{% endhighlight %}


**2.** Edit your ```_config.yml``` file. Simply add the line 

{% highlight text %}
highlighter: rouge
{% endhighlight %}

which means that you will now use Rouge as your syntax highlighter. 

**That's it, good luck!**


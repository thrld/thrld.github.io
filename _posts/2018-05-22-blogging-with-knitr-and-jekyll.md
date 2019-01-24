---
layout: post
title: "Blogging with knitr and Jekyll"
date: 2018-05-22
section: blog
output: html_document
---



I thought I'd try something new today. What you are looking at is a simple .Rmd file (in fact, this is the boring example `R` opens by default). However, the great thing about this is that our friend [yihui](https://github.com/yihui/knitr-jekyll) came up with a way to completely automate the process of turning  my underlying .Rmd into **this very blog post** (an html file) -- head, footer and liquid tags for Jekyll included. This not only allows me to write blog posts in `R`, but also to edit them on the fly. Theoretically, you could even use `servr::jekyll()` to directly look at the html file in your `R` viewer, but I personally prefer to simultaneously call `jekyll serve --watch` from my cmd and use my browser instead. They work nicely together -- `R` will update the corresponding md file once make changes in the Rmd, and Jekyll will realize and update the corresponding html. Simply refresh the browser and you're done.

Naturally, the syntax highlighting provided by IDEs like `Atom`, `RStudio`, or `Jupyter Notebook` does not magically transfer to HTML. Some extra work is needed to achieve this. Some people have created custom (Sassy) CSS files with styling rules to make their code look more familiar on their webpages (cf. the contributions by [Andy South](https://github.com/AndySouth/andysouth.github.io/blob/master/_scss/_highlights.scss) or [Yihui Xie](https://github.com/yihui/knitr-jekyll/blob/gh-pages/_sass/_syntax-highlighting.scss)). However, I personally prefer [Rouge](http://rouge.jneen.net/), an off-the-shelf solution written in Ruby. It is easy to use and produces visually pleasing results.

<hr>

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


{% highlight r %}
summary(cars)
{% endhighlight %}



{% highlight text %}
     speed           dist       
 Min.   : 4.0   Min.   :  2.00  
 1st Qu.:12.0   1st Qu.: 26.00  
 Median :15.0   Median : 36.00  
 Mean   :15.4   Mean   : 42.98  
 3rd Qu.:19.0   3rd Qu.: 56.00  
 Max.   :25.0   Max.   :120.00  
{% endhighlight %}

## Including Plots

You can also embed plots, for example:

<img src="/figure/source/2018-05-22-blogging-with-knitr-and-jekyll/pressure-1.png" title="plot of chunk pressure" alt="plot of chunk pressure" style="display: block; margin: auto;" />

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

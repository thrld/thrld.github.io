---
layout: post
title: "Blogging with knitr and Jekyll"
date: 2016-05-22
section: blog
output: html_document
---



I thought I'd try something new today. What you are looking at is a simple Rmd file (in fact, this is the boring example `R` opens by default). However, the great thing about this is that our friend [yihui](https://github.com/yihui/knitr-jekyll) came up with a way to completely automate the process of turning the Rmd into **this very blog post** (an html file) -- head, footer and liquid tags for Jekyll included. This not only allows me write blog posts in `R`, but also to edit on the fly. Theoretically, you could even use `servr::jekyll` to directly look at the html file in your `R` viewer, but I personally prefer to simultaneously call `jekyll serve --watch` from my cmd and use my browser instead. They work nicely together -- `R` will update the corresponding md file once make changes in the Rmd, and Jekyll will realize and update the corresponding html. Simply refresh the browser and you're done. I'm really happy with this.

**Next steps:** People have come up with styling rules to make the R code look more *familiar* (i.e., like classic `knitr` documents). However, these are usually provided as **Sassy** CSS files (cf. [here](https://github.com/AndySouth/andysouth.github.io/blob/master/_scss/_highlights.scss) or [here](https://github.com/yihui/knitr-jekyll/blob/gh-pages/_sass/_syntax-highlighting.scss)), which finally gives me a reason to check that out. I understand that it's mainly used to make your CSS code look less overwhelming. In this case, both authors use *nesting* and so-called *mixins* to keep their code clean. 

I hope coloring this up a little won't be to complicated. But see for yourselves, I quite like the result below already:   
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

<img src="/figure/source/2016-05-22-blogging-with-knitr-and-Jekyll/pressure-1.png" title="plot of chunk pressure" alt="plot of chunk pressure" style="display: block; margin: auto;" />

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

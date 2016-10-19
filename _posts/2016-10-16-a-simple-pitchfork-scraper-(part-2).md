---
layout: post
title: "A Simple Pitchfork Scraper (Part 2)"
date: 2016-10-16
section: blog
output: html_document
---

In the first part of the tutorial, I showed how to retrieve content from a [Pitchfork article](http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page=1). The content I was interested in was hidden in large amounts of text but we can regain control and structure it using some web scraping tricks in Python. I saved the resulting data set as an Excel file.

This is a fairly short post, nothing fancy. All I want to do is load the data into R and show you a powerful package to create tables in markdown from within an .Rmd file. This blog post is written in R. If you're interested in doing something similar yourself, check out my post on **Blogging with knitr and Jekyll**.

Let's get started:

{% highlight r %}
# load packages
require(xlsx)
require(knitr)
{% endhighlight %}


{% highlight r %}
# load excel document created in Python
setwd("path/to/excel/file")
res <- read.csv(file = "pitchfork_results.csv", header = FALSE)
{% endhighlight %}


{% highlight r %}
# set column names and take a look
colnames(res) <- c("Rank", "Artist", "Album", "Label", "Track", "Link")
head(res, n = 5L)
{% endhighlight %}



{% highlight text %}
##   Rank        Artist                      Album             Label
## 1   50          Gaze                  Mitsumeru                 K
## 2   49     Excuse 17 Such Friends Are Dangerous   KILL ROCK STARS
## 3   48 Damien Jurado                   Maraqopa SECRETLY CANADIAN
## 4   47  Youth Lagoon    The Year of Hibernation   FAT POSSUMLEFSE
## 5   46  The Spinanes          Arches and Aisles           SUB POP
##                           Track
## 1                    Jelly Bean
## 2 This Is Not Your Wedding Song
## 3                Working Titles
## 4                            17
## 5                  Kid in Candy
##                                          Link
## 1 https://www.youtube.com/watch?v=KkUD6UftGRM
## 2 https://www.youtube.com/watch?v=7wIQfNWnbS4
## 3 https://www.youtube.com/watch?v=Tn63pG-k2Hg
## 4 https://www.youtube.com/watch?v=b4_x063rhX4
## 5 https://www.youtube.com/watch?v=XDnHAn3fJww
{% endhighlight %}

Last but not least, let's create a markdown list that will look nice on a webpage. I am only including the top 5 because I don't want to reprint all of Pitchfork's work on my own site.


{% highlight r %}
# produce a beatiful markdown list
kable(tail(res[,-4], n = 5L), row.names = FALSE, format = "markdown", longtable = TRUE)
{% endhighlight %}



| Rank|Artist          |Album                           |Track            |Link                                        |
|----:|:---------------|:-------------------------------|:----------------|:-------------------------------------------|
|    5|Built to Spill  |There’s Nothing Wrong With Love |Big Dipper       |[https://www.youtube.com/watch?v=f7ysRps-iYA](https://www.youtube.com/watch?v=f7ysRps-iYA) |
|    4|Modest Mouse    |The Lonesome Crowded West       |Trucker’s Atlas  |[https://www.youtube.com/watch?v=Gl9vGHVT_Xs](https://www.youtube.com/watch?v=Gl9vGHVT_Xs) |
|    3|The Microphones |The Glow, Pt. 2                 |The Glow Pt. 2   |[https://www.youtube.com/watch?v=9Sgq8l1m9W0](https://www.youtube.com/watch?v=9Sgq8l1m9W0) |
|    2|Sleater-Kinney  |Dig Me Out                      |Buy Her Candy    |[https://www.youtube.com/watch?v=gPyDkzZEw3w](https://www.youtube.com/watch?v=gPyDkzZEw3w) |
|    1|Elliott Smith   |Either/Or                       |Between the Bars |[https://www.youtube.com/watch?v=hPD-a1FjUtU](https://www.youtube.com/watch?v=hPD-a1FjUtU) |

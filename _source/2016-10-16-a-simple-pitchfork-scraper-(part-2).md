---
layout: post
title: "A Simple Pitchfork Scraper (Part 2)"
date: 2016-10-16
section: blog
output: html_document
---

In the first part of the tutorial, I showed how to retrieve content from a [Pitchfork article](http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page=1). The content I was interested in was hidden in large amounts of text but we can regain control and structure it using some web scraping tricks in Python. I saved the resulting data set as an Excel file.

This is a fairly short post, nothing fancy. All I want to do is load the data into R and show you a powerful package to create tables in markdown from within an .Rmd file. This blog post is written in R. If you're interested in doing something similar yourself, check out my post on **Blogging with knitr and Jekyll**.

Let's get started and read in the data and set the column names


```r
# load packages
require(xlsx)
require(knitr)
```


```
## Warning in file(file, "rt"): cannot open file 'pitchfork_results.csv': No
## such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```


```r
# load excel document created in Python
setwd("path/to/excel/file")
res <- read.csv(file = "pitchfork_results.csv", header = FALSE)
```


```r
# set column names and take a look at the result
colnames(res) <- c("Rank", "Artist", "Album", "Label", "Track", "Link")
```

```
## Error in colnames(res) <- c("Rank", "Artist", "Album", "Label", "Track", : object 'res' not found
```

```r
head(res, n = 5L)
```

```
## Error in head(res, n = 5L): error in evaluating the argument 'x' in selecting a method for function 'head': Error: object 'res' not found
```

Last but not least, let's create a markdown list that will look nice on a webpage. (I am only including the top 5 because I don't want to reprint all of Pitchfork's work on my own site.)


```r
# produce a beatiful markdown list
kable(tail(res[,-4], n = 5L), row.names = FALSE, format = "markdown", longtable = TRUE)
```

```
## Error in tail(res[, -4], n = 5L): error in evaluating the argument 'x' in selecting a method for function 'tail': Error: object 'res' not found
```

---
layout: post
title: "A Simple Pitchfork Scraper (Part 1)"
date: 2016-10-16
section: blog
---

The winter semester starts tomorrow, so I thought before I have to do actual work again I'll take half an hour on a rainy Sunday afternoon to do some *web scraping*, i.e. extracting information from websites. Simple human copy-and-pasting quickly becomes too time-consuming and even with a moderate amount of data it might be worth looking into how to automate this process. This is especially true if the website we are collecting the information from is updated every now and again.

Have a look at [this webpage](http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page=1), or actually, collection of webpages that contain a ranking of *The 50 Best Indie Rock Albums of the Pacific Northwest*. As usual, there is a lot of unnecessary information and clicking involved. I wrote a [wrapper](https://en.wikipedia.org/wiki/Wrapper_(data_mining)) that extracts the information we're actually interested in for each entry: rank, artist name, album name, label name, name of the song that Pitchfork suggests we should check out, and a youtube link to this song. 

Here is a simple Python script that takes care of the web scraping task using selenium.  

First, we define a helper function that will be used below.
{% highlight python linenos %}
# define helper function 
def find_between(s, first, last):
    try:
        start = s.index(first) + len(first)
        end = s.index(last, start)
        return s[start:end]
    except ValueError:
        return ""
{% endhighlight %}
    
Then we load the necessary modules.
{% highlight python linenos %}
# import
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

from bs4 import BeautifulSoup as BS # for get_text()
{% endhighlight %}

Now let's set up everything we need for the web scraping task:
{% highlight python linenos %}
# set up the browser -- executable_path points to the location where phantomjs is stored
browser = webdriver.PhantomJS(executable_path=r'C:/Users/johndoe/Documents/automateWork/phantomjs-2.1.1-windows/bin/phantomjs.exe')

# define result list
res = []

# define urls (5 pages -> loop)
url_root = 'http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page='
url_list = []
for i in range(1,6):
        url_list.append(url_root + str(i))
{% endhighlight %}

What follows is the actual scraper:
{% highlight python linenos %}
# get content from all 5 url's:
for url in url_list:        
    browser.get(url) #This will open a Firefox window on your machine

    try:
        element = WebDriverWait(browser,10).until(EC.presence_of_element_located((By.CLASS_NAME,"year-end-review")))
    finally:
        # find all entries of ranking
        elements = browser.find_elements_by_class_name("year-end-review")  
        # dissect
        for element in elements:
            try:
                rank = element.find_element_by_class_name("rank").text 
                print("rank: ", rank)
                label = element.find_element_by_class_name("label-list").text
                print("label: ", label)
                artist = element.find_element_by_class_name("artist-list").text
                print("artist: ", artist)
                album = element.find_element_by_class_name("work-title").text
                print("album: ", album)
                blurb = element.find_element_by_class_name("blurb-text")
                # ".//", not "//" -- you need to be explicit, otherwise selenium will ignore 
                # that you specified the web elements blurb/alles as parents! 
                alles = blurb.find_elements_by_xpath(".//*[contains(text(), 'Listen:')]/../..")[-1]
                # the line above finds the parent of the parent for all elements that contain 
                # "Listen:" and selects the last hit that was encountered while scanning blurb
                herz = alles.find_elements_by_xpath(".//a")[-1]
                link = herz.get_attribute("href")
                info = BS(herz.get_attribute("innerHTML")).get_text().strip()
                artist2 = info.split(",")[0]
                track = find_between(info, "“", "”")
                print(artist2 == artist)
                res.append([rank, artist, album, label, track, link])
            except:
                continue
browser.quit()
{% endhighlight %}

That's it! Now we can print the results...
{% highlight python linenos %}
print(res)
{% endhighlight %}

... and export to csv:
{% highlight python linenos %}
# write list to Excel
    # note that "wb" has to be changed to "w" for Python 3 compatibility
    # in Python 3, newline parameter needs to be specified, otherwise you get extra lines!
import csv

with open("pitchfork_results.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerows(res)
{% endhighlight %}

The second part of this tutorial shows you how to read the data into R.
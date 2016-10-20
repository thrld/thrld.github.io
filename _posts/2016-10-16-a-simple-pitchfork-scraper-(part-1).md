---
layout: post
title: "A Simple Pitchfork Scraper (Part 1)"
date: 2016-10-16
section: blog
---

The winter semester starts tomorrow, so I thought before I have to do actual work again I'll take half an hour on a rainy Sunday afternoon to do some *web scraping*, i.e. extracting information from websites. Simple human copy-and-pasting quickly becomes too time-consuming and even with a moderate amount of data it might be worth looking into how to automate this process. This is especially true if the website we are collecting the information from is updated every now and again.

Have a look at [this webpage](http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page=1), or actually, collection of webpages that contain a ranking of *The 50 Best Indie Rock Albums of the Pacific Northwest*. As usual, there is a lot of unnecessary information and clicking involved. I wrote a [wrapper](https://en.wikipedia.org/wiki/Wrapper_(data_mining)) that extracts the information we're actually interested in for each entry: rank, artist name, album name, label name, name of the song that Pitchfork suggests we should check out, and a youtube link to this song. 

Here is a simple Python script that takes care of the web scraping task using the extension *selenium*. Let's call it *pitchf_scraper.py* -- or *pitchf_scraper.ipynb*, if you decide to work in IPython Notebook (now called [Jupyter Notebook](http://jupyter.org/)).  

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
browser = webdriver.PhantomJS(executable_path=r'C:/path/to/the/file/phantomjs.exe')

# define result list
res = []

# define urls (5 pages -> loop)
url_root = 'http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page='
url_list = []
for i in range(1,6):
    url_list.append(url_root + str(i))
{% endhighlight %}

What follows is the actual scraper:<sup><a href='#fn1' id='ref1'>1</a></sup>
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

The following will be important for the third part of the tutorial. Let's save a subset of the results using *pickle* to be able to re-import them later. To be precise, I extract the *videoID* from the youtube link to each video. Note that the links are always at the fifth<sup><a href='#fn2' id='ref2'>2</a></sup> position (the data are structured now, like a data.frame in R) and youtube links are standardized (the videoID can always be found at the same position within the link). Here is one example how one could do it:

{% highlight python linenos %}
# save links only as pickle object
links = list()
for entry in res:
    links.append(entry[5][32:43])
pickle.dump(links, open("pitchfork_video_ids.p", "wb"))
{% endhighlight %}

The [second part](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-2)) of this tutorial demonstrates how to read the data into R and create beautiful markdown tables within seconds. 

In [part 3](../../../../../2016/10/19/a-simple-pitchfork-scraper-(part-3)) I show you how to automatically make a Youtube playlist out of these 50 videos using the [Youtube Data API](https://developers.google.com/youtube/v3/) and the [Google API Client Library for Python](https://developers.google.com/api-client-library/python/). 

<hr>
*Footnotes:*

<a name="fn1">1</a>: Note that Pitchfork didn't manage their tags in a very concise way, this is why I had to play around a little with XPath in the code -- not very nice, I know.. Check out [this link](http://www.w3schools.com/xml/xpath_syntax.asp) in case you're not familiar with the XPath syntax. <sup><a href='#ref1' title='Jump back to footnote 1 in the text.'>↩</a></sup>

<a name="fn2">2</a>: Be careful -- unlike R, Python is zero-indexed. This means that you select the first element of the vector *v* using the syntax `v[0]`. <sup><a href='#ref2' title='Jump back to footnote 2 in the text.'>↩</a></sup>
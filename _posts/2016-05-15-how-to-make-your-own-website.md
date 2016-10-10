---
layout: post
title: "How to make your own website"
date: 2016-05-15
section: blog
---

Okay, imagine you have *zero* web development skills but still think it would be cool to have your own website. I get that, I feel the same. This blog post was written to give you an overview and direct you to helpful sources of information. For now, this will just be a basic rundown of the different steps you need to take, but I intend to expand on this in the future.

### The basic steps to take

1. **Hosting.** If you're like me and you don't want to pay any money for this, I recommend you create a GitHub account and use [GitHub Pages](https://pages.github.com/) to host your website directly from your GitHub repository. You will have to learn the basics of how the version control system *Git* works and [get a GitHub account](https://github.com/join). I highly recommend you get familiar with using the git shell for the basic stuff to do with repositories (like pushing, pulling, cloning, forking). As an alternative, you can use [GitHub Desktop](https://desktop.github.com/), but I really think that somewhere down the line there will be no way around using the command line.

2. **Creating content.** Ideally, you know a little bit of HTML and CSS. If not, check out [W3schools](http://www.w3schools.com/) and learn the most important stuff in like one day (really!). The same holds for JavaScript, but you might not even need that because there are a lot of helpful tools out there to help you out if you don't have time to learn 10 different languages. Now, once it comes down to creating content, you basically have two choices: <br /><br /> 
1) You write everything by hand. This means you create an html file for each page, set up the links, create styling rules using CSS, and so on. (Which is totally doable if you're not aiming a lot higher than I did with my page. In fact, this page started out like that, but I decided to switch to Jekyll just to learn something new.) <br /><br />
2) Jekyll? Yes, [Jekyll](https://jekyllrb.com/). That's your second choice. Jekyll helps you out with the static HTML stuff. If you use Windows like me, installing it is a little more complicated but don't be afraid, fellow Windows users, [it can be done](http://jekyll-windows.juthilo.com/). The main point why you might want to switch to Jekyll instead of writing everything up by hand is that Jekyll allows you to define templates (or *layouts*, as Jekyll calls them), so that you don't have to repeat yourself all the time. See how my webpage, for example, has a head and a footer that appear on every page? Or have you maybe noticed that all pages are styled in the same slightly boring way? This is Jekyll's work. You basically tell it what the fixed content should be and specify where you want to insert some flexible content (a blog post, your latest coding success, etc.). Given all that information, Jekyll will then go ahead and put together the actual HTML files that will make up your webpage. You push the results up to GitHub and a few seconds later, everything is live on the web. Cool, huh? <br /><br />
*(As an aside, I should probably mention that there are alternatives to Jekyll out there which might become more interesting as your project grows bigger. Kieran Healy switched to [Hugo](http://gohugo.io/) two years ago and [seems very happy](https://kieranhealy.org/blog/archives/2014/02/24/powered-by-hugo/), especially concerning its speed.).* <br /><br />

### Some further reading

I realize that the way this is written up at the moment is not particularly helpful. If you need more information to get you started, you should **check out these posts** - they basically assume no prior knowledge). 

1. Read [this post](http://jmcglone.com/notes/2014/05/03/using-github-to-create-and-host-a-personal-website) by Jonathan McGlone if you are currently using Wordpress (or something similar) and wonder what advantages the whole DIY thing might have. 

2. And here's Jonathan again, this time telling you [how to actually do it](http://jmcglone.com/guides/github-pages/) if you are interested. 

3. Another [tutorial](https://24ways.org/2013/get-started-with-github-pages/) by Anna Debenham. She goes a little more into detail about styling, which I found helpful. 

4. Here is a [short article](http://alistapart.com/article/responsive-web-design) about how to make your webpage *responsive* to the user's device.  

5. Here is a [post from perfectly random](http://www.perfectlyrandom.org/2014/06/29/adding-disqus-to-your-jekyll-powered-github-pages/) that explains how to get the commenting system *Disqus* (cf. below) to work on your page.
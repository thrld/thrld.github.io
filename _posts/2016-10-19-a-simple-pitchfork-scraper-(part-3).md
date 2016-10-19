---
layout: post
title: "A Simple Pitchfork Scraper (Part 3)"
date: 2016-10-19
section: blog
output: html_document
---

Before continuing, let's quickly recap what has happened so far. In the [first part](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-1)) of this tutorial, I showed how to retrieve content from a [Pitchfork article](http://pitchfork.com/features/lists-and-guides/9932-the-50-best-indie-rock-albums-of-the-pacific-northwest/?page=1) and turned the information of interest into a structured data frame. The [second part](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-2)) of this tutorial was a lot easier to digest. It demonstrated how to read the data into R and create beautiful markdown tables within seconds using only a couple lines of code. 

Now let's turn to something slightly more complicated again. You've probably heard of APIs (short for: *application programming interface*) before. In this post, I will show you how to create a new Youtbe playlist and add videos to it. Since we already scraped the Youtube links for a total of 50 songs in the [first part](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-1)), let us **not** do all this via point-and-click (I'm sure you wouldn't need a tutorial for this anyway). Much rather, I'll show you how to do this from within Python using the [Youtube Data API](https://developers.google.com/youtube/v3/) and the [Google API Client Library for Python](https://developers.google.com/api-client-library/python/).

**Here is what you need to do:**

1. Get a Google account. This includes a Google e-mail address. **Important:** Use this e-mail address to sign up for all that follows.
1. Sign up for a Youtube account.
1. Sign in at [Google APIs](https://console.developers.google.com/apis) using your Google account from 1. 
1. Before you can get started, the site will ask you to *create a project*. Now you need to tell Google which of its numerous services you are planning to use in your project. At the very top of your *Dashboard* click on *Enable API*, then select the *YouTube Data API*. Click on *Enable*, at the very top.
1. From within the project you have just created, generate an API key by clicking on *Credentials* (left bar). Click on the drop-down menu *Create credentials* and select *API Key*. 
1. For what we are planning to do, a simple API key is not enough. We also need a so-called *OAuth 2.0 client ID* which takes care of more complicated authentication processes. Click on the *Create Credentials* drop-down menu again and select *OAuth Client ID*. Under *Application type*, select *Other*. Pick a name you like and then click *Create*. A pop-up window appears and shows you your client ID and client secret. You can copy and save these somewhere if you want, but this is not mandatory. You'll see why in the next step.
1. Your ID should now be listed under *OAuth 2.0 client IDs* right below the *API keys*. If you can't see any of this, maybe you have moved away from the *Credentials* tab in the meantime. Just click on *Credentials* in the sidebar on the left and check again. You should see your client ID now. On the very right, there should be three small icons -- a pencil, a trash can, and a download symbol (from left to right). Click on the *download symbol* and download the JSON file. 
1. Now go to the folder you have used as a project folder on our local machine so far. This is where you put the Python file from the [first part](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-1)). Accordingly, the pickle file containing the list of Youtube links we generated at the end of part 1 should also live there. Now rename the JSON object you downloaded in the last step (it has a very long name starting with *client_secret_...*) to *client_secrets.json* and put it in the project folder together with the other files. The renaming is important because I used this name to reference the JSON file in the Python script you find below. Once again, note that *client_secrets.json* must be saved under **this very name** and **in the same directory** as *make_pitchf_playlist.py*, the Python file that creates the playlist (see below).
 
If you're running into problems during the authentication progress, I recommend you use Google's [API Explorer](https://developers.google.com/apis-explorer/?hl=en_US#p/youtube/v3/). It will give you more specific error messages than Python. Let me give you an example. I had everything set up but completely forgot to sign up for an actual Youtube account. Executing *make_pitchf_playlist.py* would always return an *HTTP error 401 (Unauthorized)*. It took me 20 minutes of pointless checking and re-checking my OAuth credentials to realize that I hadn't signed up for a Youtube account yet (since we're doing things with Youtube, a Google account does of course not suffice here). Had I used the above-mentioned [API Explorer](https://developers.google.com/apis-explorer/?hl=en_US#p/youtube/v3/), it would have told me something like *'I'm glad you got your OAuth 2.0 to work but how exactly do you want to create a Youtube playlist without even owning a Youtube account?'* 

But let's start from the beginning -- here is my input to make the request in the [API Explorer](https://developers.google.com/apis-explorer/?hl=en_US#p/youtube/v3/): 

1. Activate the toggle in the top right corner to authorize your request using OAuth 2.0.
2. The **part** parameter should be: *snippet,status*. All other parameters can be left blank.
3. The **Request body** should read something like:<sup><a href='#fn1' id='ref1'>1</a></sup> 

{% highlight text %}{% raw %}
{
 "snippet":
 {
  "title":"My first playlist"
 }
  "status":
 {
  "privacyStatus":"private"
 }
}  
{% endraw %}{% endhighlight %}

And here is what came back: Instead of just saying *401 Unauthorized* (like Python did), the API Explorer shows you the whole JSON object you get back which looked something like this:
{% highlight text %}{% raw %}
{
 "error": {
  "errors": [
   {
    "domain": "youtube.header",
    "reason": "youtubeSignupRequired",
    "message": "Unauthorized",
    "locationType": "header",
    "location": "Authorization"
   }
  ],
  "code": 401,
  "message": "Unauthorized"
 }
}   
{% endraw %}{% endhighlight %}

And there were the magic words: *youtubeSignupRequired*... Ok, so I created a Youtube account, re-executed the request and had more success this time -- status code *200*, all good. I checked my new Youtube account, and -- success again -- an empty playlist, just what I wanted. Then I re-ran *make_pitchf_playlist.py* and that finally worked, too. 

Talking about running this -- there is one last **very important** thing to say before we jump into the code. It needs to be saved as a *.py* file which must then be called from the command line. This might strike you as odd. I personally also like working with *Ipython Notebook* (now called [Jupyter Notebook](http://jupyter.org/)) better, because it lets you execute single statements or code blocks much more easily. In case you are wondering why we now need to resort to the command line -- this is due to the complicated authentication progress that involves the OAuth 2.0 client ID. Starting from the file *client_secrets.json*, we run through multiple steps until Youtube finally believes that we are the owner of our account. I will not go into more detail right now, but might add some paragraphs later if I find the time. 

So what does this mean with respect to file organization? As I've said already, the least error-prone way of doing this is to have one single project folder and work from there. To be more precise, this code will **only work if** your project folder contains the following files:

1. *pitchfork_video_ids.p* -- the pickle file containing the list of Youtube links (cf. end of [part 1](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-1))). 
2. *make_pitchf_playlist.py* -- the Python file from below.
3. *client_secrets.json* -- created on [Google APIs](https://console.developers.google.com/apis), see above.

If you took care of all this, open the command line tool. If you don't know where to find it, just search for *cmd* on your machine. Then cd to your project folder and run *make_pitchf_playlist.py*.<sup><a href='#fn2' id='ref2'>2</a></sup>

{% highlight text %}
cd "C:\Users\path\to\my\project\folder"
python make_pitchf_playlist.py
{% endhighlight %}

Any questions (please say no!). Okay! This means that we are now ready to dive into the code. What we have here, basically, are three functions:

1. *get_authenticated()* deals with authentication.
1. *create_playlist()* creates a new playlist in the authorized user's channel.
1. *add_videos_to_playlist()* uses the pickle file from [part 1](../../../../../2016/10/16/a-simple-pitchfork-scraper-(part-1)) and the helper function *add_video_to_playlist()* to populate the playlist using the links we scraped from the Pitchfork article.

The code looks like this: **make_pitchf_playlist.py**

{% highlight python linenos %}
#!/usr/bin/python

# Call from command line: python make_pitchf_playlist.py 
  # Note that cmd call takes no further arguments because 
  # get_authenticated() will fail otherwise as it relies
  # on argparser.parse_args().

import httplib2
import os
import sys
import pickle

from apiclient.discovery import build
from apiclient.errors import HttpError
from oauth2client.client import flow_from_clientsecrets
from oauth2client.file import Storage
from oauth2client.tools import argparser, run_flow

CLIENT_SECRETS_FILE = "client_secrets.json"

# This variable defines a message to display if the CLIENT_SECRETS_FILE is
# missing.
MISSING_CLIENT_SECRETS_MESSAGE = "WARNING: Please configure OAuth 2.0"

# This OAuth 2.0 access scope allows for full read/write access to the
# authenticated user's account.
YOUTUBE_READ_WRITE_SCOPE = "https://www.googleapis.com/auth/youtube"
YOUTUBE_API_SERVICE_NAME = "youtube"
YOUTUBE_API_VERSION = "v3"

# This function deals with getting authentication.
# Later, I will call get_authenticated() from within functions 
# that require authentication, i.e.:
#    -> create_playlist()
#    -> add_video_to_playlist
#    -> add_videos_to_playlist
def get_authenticated():
  flow = flow_from_clientsecrets(CLIENT_SECRETS_FILE,
    message=MISSING_CLIENT_SECRETS_MESSAGE,
    scope=YOUTUBE_READ_WRITE_SCOPE)

  storage = Storage("%s-oauth2.json" % sys.argv[0])
  credentials = storage.get()

  if credentials is None or credentials.invalid:
    flags = argparser.parse_args()
    credentials = run_flow(flow, storage, flags)

  return build(YOUTUBE_API_SERVICE_NAME, YOUTUBE_API_VERSION,
    http=credentials.authorize(httplib2.Http()))


# This function creates a new playlist in the authorized user's channel.
# The returned response is a JSON object, the playlist ID is in the slot ["id"].
def create_playlist(title, description, privacyStatus):
  # get authentication using function defined above
  youtube = get_authenticated()

  response = youtube.playlists().insert(
    part="snippet,status",
    body=dict(
      snippet=dict(
        title=title,
        description=description
      ),
      status=dict(
        privacyStatus=privacyStatus
      )
    )
  ).execute()
  print("I generated a new, empty playlist:\n\tTitle: \'{0}\'\n\tDescription: \'{1}\'\n\tID: \'{2}\'".format(title, description, response["id"]))
  return(response)


# This function adds a video to an existing playlist. It requires a videoID and a playlistID.
# Note that get_authenticated() is called from within the function.
def add_video_to_playlist(videoID, playlistID):
  # get authentication using function defined above
  youtube = get_authenticated()

  print("Adding video to playlist with ID %s:" % playlistID)

  add_video_request=youtube.playlistItems().insert(
    part="snippet",
    body={
      'snippet': {
        'playlistId': playlistID, 
        'resourceId': {
          'kind': 'youtube#video',
          'videoId': videoID
        }
        #'position': 0
      }
    }
  ).execute()
  print("Video added. Video ID: %s" % videoID)


# This function adds multiple videos to an existing playlist. 
# It requires a playlistID and the file name of a pickle file that contains a list of videoID's.
# Note that get_authenticated() is called from within the function.
def add_videos_to_playlist(pickle_ids_file, playlistID):
  # get authentication using function defined above
  youtube = get_authenticated()
  
  ids = pickle.load(open(pickle_ids_file, "rb"))

  print("Adding videos to playlist with ID \'%s\':" % playlistID)
  i = 1
  for id in ids: 
    add_video_request=youtube.playlistItems().insert(
      part="snippet",
      body={
        'snippet': {
          'playlistId': playlistID, 
          'resourceId': {
            'kind': 'youtube#video',
            'videoId': id
          }
          #'position': 0
        }
      }
    ).execute()

    print("\t{0}. ID: {1}".format(i, id))
    i += 1
  print("Done. I added a total of %s videos." % str(i-1))


# Define function parameters
title = "The Pitchfork Playlist"
description = "A web scraping exercise"
privacyStatus = "public"

# Create playlist and save its ID 
playlist_response = create_playlist(title, description, privacyStatus)
playlistID = playlist_response["id"]

# Load list of video IDs
pickle_ids_file = "pitchfork_video_ids.p"

# Add videos to playlist
add_videos_to_playlist(pickle_ids_file, playlistID)
{% endhighlight %}

I should say that there are some things I'm not happy with yet. I might improve this at a later point. However, the function as presented above works and can easily be adapted to different scenarios. 

**Good luck, let me know if you run into any issues or would like anything to be explained in more detail.** 

<hr>
*Footnotes:*

<a name="fn1">1</a>: If you want the playlist to be public, simply replace *private* with *public*. Find out more about parameters and request body [here](https://developers.google.com/youtube/v3/docs/playlists/insert). <a href='#ref1' title='Jump back to footnote 1 in the text.'>↩</a>


<a name="fn2">2</a>: If you get an error while executing *python make_pitchf_playlist.py*, it might be because your system doesn't know where to look for *python.exe*. In this case, you need to find it. It might for example be in *C:/Python27* or in *C:/Python34* or somewhere else completely. The important bit is that you identify in which folder the file *python.exe* lives. Once you have done this, copy the path to this folder and search your system for *environment* (or, in German: *Systemumgebungsvariablen*. I know, a beautiful language, right?). Open it and double-click on the *PATH* variable, then click on *New* and paste in the path. Go back to the command line -- now, *which python* should give you back the path you just entered. <a href='#ref2' title='Jump back to footnote 2 in the text.'>↩</a>

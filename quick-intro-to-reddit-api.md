Quick introduction: programmatically using reddit and writing reddit bots

When you type http://reddit.com/ in your browser, your browser makes a request to reddit.com servers, and the servers respond with the frontpage. Here is what the frontpage looks to a browser. The frontpage is written using HTML, CSS and JS. While it is possible to process HTML(CSS and even JS) from your code, this is not super convenient! However, we can make requests to another URLs to get data in another format(JSON) that is easier to parse.

Here is what the frontapage looks like: http://www.reddit.com/.json

In fact, you can make those requests from terminal:

    $ curl http://www.reddit.com/.json

Parsing data in terminal isn't fun. But we can use python. Here is the first result from the front page:

    >>> import requests
    >>> user_agent = {'User-agent': 'doing some testing by /u/<your_username>'}
    >>> response  = requests.get("http://www.reddit.com/.json", headers = user_agent)
    >>> response.json()['data']['children'][0]['data']['url']
    u'http://www.nasa.gov/press-release/nasa-kepler-mission-discovers-bigger-older-cousin-to-earth'
    >>> response.json()['data']['children'][0]['data']['score']
    5963

In fact, there are a lot of different URLs that you can use. Here is the full specification that reddit implements: https://www.reddit.com/dev/api

This is called API(Application Programming Interface), because, well, it is used to program applications that use or control reddit. Reddit also imposes certain restriction on how you can use API. For example, you have to make no more than 30 requests per minutes. And you also have to tell reddit who is using the API in user-agent(user-agent is a special string passed along with a request).

You can make these requests from pretty much any language(python, bash, java, c++, ruby, haskell, etc, etc). However, people wrote a bunch of libraries to simplify using the API. Here is the full list: https://github.com/reddit/reddit/wiki/API-Wrappers


For python, there is [PRAW](https://praw.readthedocs.org/). Again, let's get the first result from the front page:


    >>> import praw
    >>> r = praw.Reddit('doing some testing by /u/<your_username>')
    >>> list(r.get_front_page())[0].comments[0].body
    u'I guess for all of us french fry lovers, our wish has been... granted.'
    >>> r = praw.Reddit('doing some testing by /u/<your_username>')
    >>> list(r.get_front_page())[0].url
    u'http://i.imgur.com/kCT0qHF.jpg'
    >>> list(r.get_front_page())[0].score
    5600
    >>> list(r.get_front_page())[0].comments[0].body
    u'I guess for all of us french fry lovers, our wish has been... granted.'


This looks a bit nicer, and having an object with nice fields for each submission/comment is really convenient! Praw has a good starting guide, so if you'd like to start making reddit bots, read this: https://praw.readthedocs.org/en/v3.1.0/pages/writing_a_bot.html

PS: I have quite a bit of experience writing reddit bots, so I guess you can AMA about reddit api.

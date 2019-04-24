# Tweepy Bot Example

This bot is based off my Tweepy Stream Example here https://github.com/jonathanhayes/Tweepy-Twitter-Stream-Example
It's just a slightly modified version of that script.

Pay particular attention to line 168 through line 175 for the reply functionality.
The trigger keyword(like !command) is at line 51

Important to note - Twitter doesn't like it when you tweet the same message a lot. If you get a duplicate message error, 
you might want to change up tweet a bit each time.

Also, I created this handy dandy delete snippet below that will help you remove all tweets you made in the last certain amount of time.

For example, right now it is set to remove all tweets you made in the last 1 hour. If you want, you could change

```if datetime.now()-timedelta(hours=Hours) <= tweet.created_at: ```

to

```if datetime.now()-timedelta(minutes=Hours) <= tweet.created_at:```

to make it remove minutes instead

```import tweepy, datetime, time
from datetime import datetime, timedelta
from time import sleep

consumer_key = 'YourConsumerKeY'
consumer_secret = 'YourConsumerSecret'
access_token = 'YourAccessToken'
access_token_secret = 'YourAccessTokenSecret'

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

name = "Your Twitter Name"
Hours = 1 # Set how many hours you want to go back to delete


def get_tweets(api, username):
    page = 1
    deadend = False
    tweets = api.user_timeline(username, page = page)

    for i,tweet in enumerate(tweets):
        if datetime.now()-timedelta(hours=Hours) <= tweet.created_at:
            print(f'Removed \'{tweet.text}\'')
            api.destroy_status(tweet.id)
            sleep(0.5)
        else:
            deadend = True
            return
    if not deadend:
        page+=1
        time.sleep(500)

get_tweets(api, name)```

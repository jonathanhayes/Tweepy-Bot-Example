# Tweepy Bot Example

This bot is based off my Tweepy Stream Example here https://github.com/jonathanhayes/Tweepy-Twitter-Stream-Example
It's just a slightly modified version of that script.

Pay particular attention to line 168 through line 175 for the reply functionality.
The trigger keyword(like !command) is at line 51

Don't forget you can also set up multiple search filters to listen for, so you can do something like this

search = ['!command1', !command2']

and do different logic depending on what the trigger word is.

# Important

Twitter doesn't like it when you tweet the same message a lot. If you get a duplicate message error, 
you might want to change up tweet a bit each time.



# Requirement

You will need a twitter API access key. It's not hard to get. 
Sign up for one at https://developer.twitter.com/en/apply-for-access.html
Once you have your API keys, scroll 1/3 of the way down the script and look for 'AUTHENTICATION' and add your API key there.



# Delete helper

Also, I created this little delete snippet below that will help you remove all tweets you made in the last certain amount of time for whenever you're testing your bot. This is so you don't have to keep going to twitter and manually removing your tweets. 

For example, right now it is set to remove all tweets you made in the last 1 hour. If you want, you could change

```if datetime.now()-timedelta(hours=Hours) <= tweet.created_at: ```

to

```if datetime.now()-timedelta(minutes=Hours) <= tweet.created_at:```

and set your Hours variable to like 30 for example to make it remove the last 30 minutes instead.

----------------------------------

Delete Script:

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


----------------------------------------------



Tidbit - I most definitely enjoy sharing code with others 
and helping people for free without EVER asking anything in 
return, but if you found this example useful and would like 
to buy me a cup of coffee or something, feel free to send a
dollar or two to paypal.me/jhayes88 if you'd like. 
Tips are always appreciated <3 


# KnowlesBot

Welcome to the KnowlesBot README! KnowlesBot is a **Facebook Messenger Bot** that I developed
for my roommate and I to more effectively manage living together slash the day to day things we want to do. The messenger server back end is deployed using Heroku,
but there are also other components that will work through a **Raspberry Pi**
(IE Spotify + Netflix + Youtube integration).

Also, the communication functions interfacing with the FB Messenger API were mostly
themselves (sometimes verbatim) implemented in Facebook's Node sample code. I have
taken what they provided within that sample code and tried to run with it.

The project itself, at this point in time, is in an early stage. I'm just profiling
out exactly what I want to build, and setting up Continuous Integration and Build Scripts so that I can more efficiently work on this in the months to come.

### Current Ideas to Implement in the future:

  * Timers and Reminders
  * Video streaming via **VLC** through command line on **Raspberry Pi** via **Chromecast**
  * **Netflix** streaming through **Tasker** and **Google Chrome** via **Chromecast**
  * **Spotify** streaming through **Mopidy** and **Bluetooth**
  * Weird, idiosyncratic things (like Wesley Crusher Memes, etc.)
  * Shopping Lists (probably through **Redis**, for lightweight storage that
    easily expires -- I don't care if I wanted to get milk 6 weeks ago)
  * Event/Exam Reminders (for extra courtesy towards someone studying for something)
  * Some sort of ability for friends to figure out what we're up to in an
    externally facing way (IE board game nights, parties, concerts, etc). This is
    probably going to be implemented in **Redis**.
  * Ability to query when certain establishments are open (IE ION Restaurant and
    Weshop in my use case, at Wesleyan University).

For the more personal details (IE where my roommate or I are at any point in
  time, or what we're up to, or to access our video/audio smart home functionalities),
  I'm going to implement some sort of Authentication based on ID (IE Chris or
    I can set the friends that are able to do different things), probably in **Redis**, and then the
    users will either be able to do the action, or will get a dialogue saying that
    the action itself cannot be completed.

### Setup

Note that Facebook forces you to deploy your Messenger BackEnd server out of
an external server with proper SSL certificates set up (IE it needs to use
HTTPS protocol, to prevent their messages from being piggybacked by unwieldy
individuals -- makes sense). For that reason, I use Heroku to deploy my app
externally.

One can just deploy the code using `git heroku push` (after having set up
a Heroku account, and linked Heroku to the current git repo using `heroku create`,
installing all required module dependencies using `npm install`, and setting up your environmental variables, which I outline below).

#### Environmental Variables

To properly deploy the server, you need to set up four environmental variables.
They are as follows:

`APP_SECRET` : This is your 'Application Secret', which you can obtain from the
Facebook Developer Platform. This is used to cryptographically check, via the
'crypto' module and the sha1 hashing algorithm, that the packet actually comes
from Facebook itself, and not anyone else masquerading as Facebook (looking at you,
Weibo!).

`MESSENGER_VALIDATION_TOKEN`: This is the token used by Facebook to verify your
Webhook works, initially. It can be assigned to any string -- just make sure to
copy that same string into Facebook's prompt in the Webhook confirmation dialogue
on the Facebook Developer platform. The explanation for that comes later.

`SERVER_URL`: This is just the address to which Facebook's GET and POST requests
should be directed. In my case, it's my_heroku_name.herokuapps.com/messenger. Your
URL will depend upon which address your web app is hosted on (for Heroku, it will
look very similar).

`FB_PAGE_ACCESS_TOKEN` : Generated from Facebook's App Dashboard on the Developer
Platform.

To set environmental variables on your heroku server, run `heroku config:set <VARIABLE_NAME>=value`. IE, for a `MESSENGER_VALIDATION_TOKEN` of `hello`, one
would run `heroku config:set MESSENGER_VALIDATION_TOKEN=hello`.

Then, all you need to do is 'hook up' your Webhooks on Facebook's Developer Platform,
and you should be able to run the Messenger
Backend in its current form, via `git push heroku`. Message the Bot, and it
should give you a response. If not, check your environmental variables and URLs.

Happy Hacking,

-@lacoperon
---
layout: post
title: "create-react-app w/ rails 5 --api"
date: 2017-08-11
---
# The Concept #
## Abstract ##
This is an alternative way of building web apps using an agnostic front end framework or client with a RESTful API on the application tier.  Essentially the front end is just delivered the data it needs, usually in the form of a JSON object, and the back end process simple discrete requests that alter the available resources.  From the viewpoint of the application there is no user state to capture beyond these moments that a network request is made to either the React server or the Rails server.

## Why React? ##
It doesn't rely on the standard method of having your three pillars of front end:
- HTML file
- Javascript file
- CSS file

Instead we sort of mush them all together  into one big file and manage the state by constantly re-rendering one big tree of elements.  I tried it and hated it at first but now I'm having fun!  It feels very natural to focus my energies on building reusable components instead of building out another template or getting that feeling of deja vu as I make a new form that feels *eerily* familiar.

## Why Rails? ##
[The official Rails doc says it best](http://edgeguides.rubyonrails.org/api_app.html).

First off I know it and I like it.  I like Ruby and Rails even though the *magic* can be a little too magical.  But that feeling means you probably need to stop what you're doing and read some docs/source code.  If you feel that things happen too automagikally at the command line just remind yourself that it doesn't need to be a `rails generate ...` command.  You can do all of it by hand.  You can type out those `mkdir` and `touch` commands all you want.  And you probably should.  But for an experiment into Rails 5 --api only mode this was a lot of fun.

Beyond the familiarity I can safely say that if you're building an API for the 2nd or 3rd time you'll *REALLY* start to feel the deja vu.  The business logic and names of resources are really the only things that change.  Beyond that you're always going to be putting up the same old skeleton and pumping life into it just like you did last time.


## What does this look like? ##
React sits on its own server farm or maybe just an AWS S3 bucket and responds to the user requests.  All requests relating to the business resources are sent to the API from the React client.  Because this is a truly RESTful implementation of an API the requests only need to know the bare minimum of the client state.  The API processes the client request and sends its response back to the source.  There's no need to store the Users session or state.  We may want to build a queue or cache to persist the user's state on the React front end so that they can pick up where they left off if they're interrupted but beyond that the API just exists and responds to requests.

### The 'ping' route: 'localhost:3000/api/v1/ping' ###

```ruby
../controllers/api/v1/ping_controller

class Api::V1::PingController < ApplicationController
  respond_to :json
  def ping
    render json: { message: "Hello and welcome to Helpdesk!" }, status: :ok 
  end
end
```

This is an excellent example of what most of the routes in your API will look like.  If this was a running server you would see the JSON message appear in the browser as a regular JSON object.  If you were a client device calling the API you would probably process the message somehow and make some changes to the client's state.  Or maybe just print it out.

# The Build ##

## Building the Front End ##
Just use [create-react-app](https://github.com/facebookincubator/create-react-app). If you don't know what that is just accept its the easiest way to build react apps.  If you want to really learn how the sausage is made just find an old guide from 2014 and follow that.  Once you get to `npm start` and see that beatuiful spinning react symbol you're done with the front end!

## Building the Back End ##
This one's a little trickier if you haven't had any Rails experience BUT luckily we can start small and generate *just* enough code to return a simple JSON  message to the front end.

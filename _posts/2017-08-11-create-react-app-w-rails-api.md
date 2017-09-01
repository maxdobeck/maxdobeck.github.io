---
layout: post
title: "create-react-app w/ rails 5 --api"
date: 2017-08-11
---
# The Concept #
## Abstract ##
This is an alternative way of building web apps using an agnostic front end framework or client with a RESTful API on the application tier.  Essentially the front end is just delivered the data it needs, usually in the form of a JSON object, and the back end process simple discrete requests that alter the available resources.  From the viewpoint of the application there is no user state to capture beyond these moments that a network request is made to either the React server or the Rails server.

## Why Rails? ##
[The official Rails doc says it best](http://edgeguides.rubyonrails.org/api_app.html).

First off I know it and I like it.  I like Ruby and Rails even though the *magic* can be a little too magical.  But that feeling means you probably need to stop what you're doing and read some docs/source code.  If you feel that things happen too automagikally at the command line just remind yourself that it doesn't need to be a `rails generate ...` command.  You can do all of it by hand.  You can type out those `mkdir` and `touch` commands all you want.  And you probably should.  But for an experiment into Rails 5 --api only mode this was a lot of fun.

Beyond the familiarity I can safely say that if you're building an API for the 2nd or 3rd time you'll *REALLY* start to feel the deja vu.  The business logic and names of resources are really the only things that change.  Beyond that you're always going to be putting up the same old skeleton and pumping life into it just like you did last time.


## What does this look like? ##
React sits on its own server or server farm and responds to users as they connect.  Whenever the user does something like a Save or Update to a resource an HTTP request gets sent to the API server.  The API processes the client request, interacts with some resource, and sends the response back to the source.  There's no need to store the Users session or state on the API server because this is a RESTful architecture that is stateless by design.

We may want to use a queue or cache to persist the user's state on the React front end so that they can pick up where they left off if they're interrupted but beyond that the API just exists and responds to requests.  When we use the word stateless what we mean is that no single HTTP request from the client needs to know about any previous request.  All necessary data is contained within the HTTP request: authentication for protected resources, the location of the resource, and what we want to do with it.

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

# The Build #

## Building the Front End ##
For our front end application we'll use [create-react-app](https://github.com/facebookincubator/create-react-app). This is the easiest way to build react apps.  If you want to really learn how the sausage is made just find an old guide from 2014 and follow that.  If you followed the most recent guide to get your create-react-app app up and running you'll run `$ npm start` and see a brand new React application!

## Building the Back End ##
Now we need to generate the Rails API using a few of Rail's builtin commands.  This will be somewhat similar to the create-react-app process in that you'll supply a name and a few optional paramaters to the command line.  But unlike the create-react-app we'll also be able to generate most of the resources we will use.  A resource here is defined as something that a user would be interested in creating, viewing, or modifying.  We can further define a resource as being the final path in the HTTP request to the API.  

For example if we have an API that lets you make and modify hotel reservations and you wanted to delete your reservation it would look like: 

```
DELETE https://www.hotelApi.com/reservations/2017-08-25
```  

The method of the request is `DELETE`, the hostname is `www.hotelApi.com`, and the path(resource or URI) is `/reservations/2017-08-25`.  These components makeup the request header that React will send to our API.  In response to this request the API could return a `true` or `false`, a confirmation, or an error saying this user doesn't have access to a protected resource (it would be chaos if anyone could delete anyone else's hotel reservations).  If you reference other modern APIs though the typical thing to do is to return a JSON message.  Since this is a `DELETE` operation  it will probably just be a simple confirmation so that the React application can say something like, "Successfully deleted!".

## Generating an API ##
The below assumes you have Rails 5 installed.  Lets build an API that can help us manage tickets submitted to the IT department.

```
$ rails new HelpdeskApi --api --database=postgresql
```
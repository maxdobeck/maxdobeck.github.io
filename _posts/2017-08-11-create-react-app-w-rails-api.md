---
layout: post
title: "create-react-app w/ rails 5 --api"
date: 2017-08-11
---

React sits on the its own set of server farms or maybe just an AWS S3 bucket and responds to the user interactions.  Meanwhile all requests relating to the business resources are sent to the API.  Because this is a truly RESTful implementation of an API the requests only need to know the bare minimum of the client state.  The API processes the client request and sends its response back to the source.  There's no need to store the Users session or state.  We may want to build a queue or cache to persist the user's state on the front end so that they can pick up where they left off if they're interrupted but beyond that the API just exists and responds to requests.

~~~ruby
class Api::V1::PingController < ApplicationController
  respond_to :json
  def ping
    render json: { message: "Hello and welcome to Helpdesk!" }, status: :ok 
  end
end
~~~

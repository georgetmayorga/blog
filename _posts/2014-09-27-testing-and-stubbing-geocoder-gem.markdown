---
layout: post
title: Testing and Stubbing the Geocoder gem
---

The [geocoder](https://github.com/alexreisner/geocoder) gem is very useful tool in getting location information about your users.
Here's a bit about testing and stubbing HTTP requests, and some of the problems when
pulling IP addresses in `test` and `development` environments.

Here's my test:

    # spec/features/user_location_spec.rb

    require 'spec_helper'

    feature "Users location is added to profile" do
      scenario "while they're filling out the profile page" do
        user = FactoryGirl.create(:user)
        visit root_path

        sign_in_as user

        expect(page).to have_text 
          "Location: Cambridge, Massachusetts"
      end
    end

I'm asserting that I'll have `Cambridge, Massachussetts` printed on the page.
There are 2 parts to this process: setting, and lookup.

## Setting
I want to get this information from the IP address. I'm collecting that information
everytime a user logs on, here:

    # app/controllers/sessions_controller.rb

    def create
      user = authenticate_session(session_params)

      if sign_in(user)
        current_user.ip_address = request_location
        current_user.save
        redirect_to root_path
      else
        @user = User.new
        render [:new, :user]
      end
    end

We want to avoid passing `Geocode.search` (coming up) the IP address `127.0.0.1` (which is our IP in `test` and `development`)
 because it won't even make a HTTP request that we can stub, it will just return nil. So instead we
do some situation handling here with `request_location` which I define in `ApplicationController`:

    # app/controllers/application_controller.rb

    def request_location
      if Rails.env.development? || Rails.env.test?
        "76.24.18.47"
      else
        request.location.data["ip"]
      end
    end

Now that we've changed the IP address for `development` and `test` we're ready to make our Geocode request.

    # app/models/user.rb

    geocoded_by :ip_address
    after_validation :geocode

In `app/controllers/sessions_controller.rb` when we call `current_user.save` this invokes
the `after_validation :geocode` which pulls the `ip_address` column from the current user
and sets the `latitude` and `longitude` fields in the database. It does this by making
and external HTTP request that we need to sub (because we've turned on Webmock to disallow
HTTP requests). Here's how we configured [webmock](https://github.com/bblimke/webmock):

    # spec/spec_helper.rb

    require 'webmock/rspec'
    WebMock.disable_net_connect!(allow_localhost: true)

    RSpec.configure do |config|
      config.before(:each) do
        stub_request(:get, "http://freegeoip.net/json/76.24.18.47").
          with(:headers => {'Accept'=>'*/*', 'User-Agent'=>'Ruby'}).
          to_return(:status => 200, :body => '{"ip":"76.24.18.47","country_code":"US","
            country_name":"United States","region_code":"MA","region_name":"Massachusetts",
            "city":"Cambridge","zipcode":"02138","latitude":42.38,"longitude":-71.1329,
            "metro_code":"506","area_code":"617"}', :headers => {})
      end
    end

You can see that we're stubbing a `get` request to `"http://freegeoip.net/json/76.24.18.47"`,
which contains the IP address we fed it. The other thing you can see is the `:body` key
is pointing to a value that is a `json` response. Geocode gives is a cool command line
helper to get the response we need to stub. By typing `geocode -j 'you-search-term'`
you can get the `json` hash your app would get, allowing you to succesfully stub it. We passed it
our IP address to make sure it will stub the information our test expects.

Now what if we want to look up a users location to show them?

## Lookup
Now that I have the information in the database, I want to pull it and show it on a users
dashboard. Here is how I'll do that:

    # app/views/dashboards/show.html.erb

    Location: <%= @location %>


    # app/controllers/dashboards_controller.rb

    @location = current_user.location

VERY simple. Let's see what `location` looks like for `current_user`.

    def location
      result = Geocoder.search(ip_address).first
      result.city + ", " + result.state
    end

Passing the `Geocoder.search` function our IP address will give us a [Geocoder::Result](https://github.com/alexreisner/geocoder#advanced-geocoding) object. We know that object responds to `.city` and `.state` and add some formatting. 
With this set up our tests run green and we're good to go!

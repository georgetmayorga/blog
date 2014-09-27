---
layout: post
title: Testing and Stubbing the Geocoder gem
---

The [geocoder](https://github.com/alexreisner/geocoder) gem is very useful tool in getting location information about your users.
Here's a bit about testing and stubbing HTTP requests, and some of the problems when
pulling IP addresses in `test` and `development` environments.

Here's my test:

```ruby
# spec/features/user_location_spec.rb

require 'spec_helper'

feature "Users location is added to profile" do
  scenario "while they're filling out the profile page" do
    user = FactoryGirl.create(:user)
    visit root_path

    sign_in_as user

    expect(page).to have_text "Location: Cambridge, Massachusetts"
  end
end
```

I'm asserting that I'll have `Cambridge, Massachussetts` on printed on the page.
There are 2 parts to this process: setting, and lookup.

## Setting
I want to get this information from the IP address. I'm collecting that information
everytime a user logs on, here:

```ruby
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
```

We want to avoid passing `Geocode.search` (coming up) the IP address `127.0.0.1` (which is our IP in `test` and `development`)
 because it won't even make a HTTP request that we can stub, it will just return nil. So instead we
do some situation handling here with `request_location` which I define in `ApplicationController`:

```ruby
# app/controllers/application_controller.rb

def request_location
  if Rails.env.development? || Rails.env.test?
    "76.24.18.47"
  else
    request.location.data["ip"]
  end
end
```

Now that we've changed the IP address for `development` and `test` we're ready to make our Geocode request.

```ruby
# app/models/user.rb

geocoded_by :ip_address
after_validation :geocode
```

In `app/controllers/sessions_controller.rb` when we call `current_user.save` this invokes
the `after_validation :geocode` which pulls the `ip_address` column from the current user
and sets the `latitude` and `longitude` fields in the database. Now what if we want to look up
a users location to show them?

## Lookup
Now that I have the information in the database, I want to pull it and show it on a users
dashboard. Here is how I'll do that:

```ruby
# app/views/dashboards/show.html.erb

Location: <%= @location %>
```


```ruby
# app/controllers/dashboards_controller.rb

@location = current_user.location
```

VERY simple. Let's see what `location` looks like for `current_user`.

```ruby
def location
  result = Geocoder.search(ip_address).first
  result.city + ", " + result.state
end
```

Passing the `Geocoder.search` function our IP address will give us a [Geocoder::Result](https://github.com/alexreisner/geocoder#advanced-geocoding) object. We know that object responds to `.city` and `.state` and add some formatting. 
With this set up our tests run green and we're good to go!

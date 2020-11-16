What is Klaviyo?
----------------

Klaviyo is a real-time service for understanding your customers by aggregating all your customer data, identifying important groups of customers and then taking action.
http://www.klaviyo.com/

What does this Gem do?
----------------------

* Track customers and events directly from your backend.
* Track customers and events via JavaScript using a Rack middleware.
* Access historical metric data.
* Manage lists and profiles in Klaviyo.

How to install?
---------------

```
gem install klaviyo
```

How to use it with a Rails application?
---------------------------------------

To automatically insert the Klaviyo script in your Rails app, add this to your environment config file or create a new initializer for it:

```ruby
require 'rack/klaviyo'
config.middleware.use "Klaviyo::Client::Middleware", "YOUR_KLAVIYO_API_TOKEN"
```

This will automatically insert the Klaviyo script at the bottom on your HTML page, right before the closing `body` tag.

To record customer actions directly from your backend, in your `application_controller` class add a method to initialize set your public and private API tokens.

```ruby
require 'klaviyo'

class ApplicationController < ActionController::Base
  Klaviyo.public_api_key = 'YOUR_PUBLIC_API_KEY'
  Klaviyo.private_api_key = 'YOUR_PRIVATE_API_KEY'
end
```

Then in your controllers where you'd like to record an event:

```ruby
Track.track('Did something important',
  email: 'john.smith@example.com',
  properties: { key: 'value' }
)
```
To add profiles to a list, call the Lists.add_to_list method and provide a list of profile objects that are identified by an email address.

Example - Adding profiles to a list:

```ruby
Lists.add_to_list('LIST_ID', profiles: [{email: 'test1@example.com'}, {email: 'test2@example.com'}])
```

To add or update the attributes of a profile, call the Profiles.update_person_attributes method and provide the profile ID and the attributes to change/add.

Example - Adding the 'LovesKlaviyo' attribute to a profile:

```ruby
Profiles.update_person_attributes('PROFILE_ID', LovesKlaviyo: true)
```

To query the Metrics API, call the get_metrics method. Accepts optional page or count keyword arguments.

Example - Requesting the 2nd page of metrics, to return a count of 100:

```ruby
Metrics.get_metrics(page: 2, count: 100)
```

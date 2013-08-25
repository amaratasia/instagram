# Instagram API

This gem is a simple and easy to use wrapper for Instagram's API.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'instagram_api'
```

And then execute:

```bash
$ bundle
```

Or install it yourself as:

```bash
$ gem install instagram_api
```

## Usage

All methods in Instagram's API require some kind of authentication. You can retrieve any public data by using just your client ID, but it is recommended to authorize yourself (and your users) using an access_token. You can get your client ID and client secret by visiting [http://instagram.com/developer](http://instagram.com/developer).

```ruby
# Instantiate a new client.
client = Instagram.client(
  :client_id     => '2bfe9d72a4aae8f06a31025b7536be80',
  :client_secret => '9d667c2b7fae7a329f32b6df17926154',
  :callback_url  => 'http://example.com/'
)

# Visit the authorization URL in your browser and login.
client.authorize_url
# => "https://api.instagram.com/oauth/authorize/?client_id=2bfe9d72a4aae8f06a31025b7536be80&redirect_uri=http://example.com/&response_type=code"

# Retrieve the code from the URL parameters and use it to get an access token.
client.get_access_token('88fb89ab65454da2a06f2c6dacd09436')
# => '1313345.3fedf64.a0fcb7f40e02fe3da50500'
```

The last method, `get_access_token`, will return your access token as well as set it for your client. You can then access any method in the API using your client.

Alternatively, after you or your user have authenticated, you can reinstantiate your client using the previously returned access token.

```ruby
client = Instagram.client(:access_token => '1313345.3fedf64.a0fcb7f40e02fe3da50500')
```

### Users API Methods

You can access various information about Instagram users by using the following methods:

```ruby
# Get a user information by id or username.
client.user(16500486)
client.user('caseyscarborough')

# Get the authenticated user's information.
client.user

# Search for a user by username.
client.search('github')

# Retrieve an authenticated user's feed.
client.feed

# Get a user's recent updates.
client.recent(16500486)

# Retrieve an authenticated user's updates.
client.recent

# Get an authenticated user's liked photos/videos.
client.liked
```

See the [Instagram User Endpoints](http://instagram.com/developer/endpoints/users/) for more information.

### Media API Methods

You can retrieve media information by using the following methods:

```ruby
# Get information about a media object by its ID.
client.media(42020)

# Search for a media item by latitude and longitude (required),
# with distance constraints (default 1000 meters).
client.media_search(:lat => "48.858844", :lng => "2.294351")
client.media_search(:lat => "48.858844", :lng => "2.294351", :distance => 2000)

# Search with Unix timestamp constraints.
client.media_search(
  :lat => "48.858844",
  :lng => "2.294351",
  :min_timestamp => 1357020000,
  :max_timestamp => 1375246800
)

# Get a list of popular media at the moment.
client.popular_media
```

See the [Instagram Media Endpoints](http://instagram.com/developer/endpoints/media/) for more information.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

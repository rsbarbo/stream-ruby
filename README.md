stream-ruby
===========

stream-ruby is a Ruby client for [Stream](https://getstream.io/).

### Installation

```bash
gem install "stream-ruby"
```


### Usage

```ruby
# Instantiate a new client
require 'stream'
client = Stream::Client.new('YOUR_API_KEY', 'API_KEY_SECRET')
# Find your API keys here https://getstream.io/dashboard/

# Instantiate a feed object
user_feed_1 = client.feed('user:1')

# Get activities from 5 to 10 (slow pagination)
result = user_feed_1.get(:limit=>5, :offset=>5)
# (Recommended & faster) Filter on an id less than the given UUID
result = user_feed_1.get(:limit=>5, :id_lt=>'e561de8f-00f1-11e4-b400-0cc47a024be0')

# Create a new activity
activity_data = {:actor => 1, :verb => 'tweet', :object => 1, :foreign_id => 'tweet:1'}
activity_response = user_feed_1.add_activity(activity_data)

# Remove an activity by its id
user_feed_1.remove('e561de8f-00f1-11e4-b400-0cc47a024be0')

# Remove activities by their foreign_id
user_feed_1.remove('tweet:1', foreign_id=true)

# Follow another feed
user_feed_1.follow('flat:42')

# Stop following another feed
user_feed_1.unfollow('flat:42')

# Batch adding activities
activities = [
    [:actor => '1', :verb => 'tweet', :object => '1'],
    [:actor => '2', :verb => 'like', :object => '3']
];
user_feed_1.addActivities(activities);

# Add an activity and push it to other feeds too using the `to` field
data = [
    :actor_id => "1",
    :verb => "like",
    :object_id => "3",
    :to => ["user:44", "user:45"]
];
user_feed_1.add_activity(data);

# Remove a feed and its content
user_feed_1.delete

# Generating tokens for client side usage
token = user_feed_1.token

# Javascript client side feed initialization
# user1 = client.feed('user:1', '{{ token }}');

# Retrieve first 10 followers of a feed
user_feed_1.followers(limit=10);

# Retrieve 10 followers of a feed starting from 11th (2nd page)
user_feed_1.followers(limit=10, page=2);

# Retrieve 10 feeds followed by user_feed_1
user_feed_1.following(limit=10);

# Retrieve 10 feeds followed by user_feed_1 starting from 11th (2nd page)
user_feed_1.following(limit=10, page=2);

# Check if user_feed_1 follows specific feeds
user_feed_1.following(limit=2, page=1, filter=['user:42', 'user:43']);

```

Docs are available on [GetStream.io](http://getstream.io/docs/).

---
title: "Implement a user profile service"
slug: "implement-a-user-profile-service-python"
hidden: true
createdAt: "2020-04-28T14:15:09.572Z"
updatedAt: "2020-04-28T14:15:09.572Z"
---
Use a **User Profile Service** to persist information about your users and ensure variation assignments are sticky. For example, if you are working on a backend website, you can create an implementation that reads and saves user profiles from a Redis or memcached store. 

In the Python SDK, there is no default implementation. Implementing a User Profile Service is optional and is only necessary if you want to keep variation assignments sticky even when experiment conditions are changed while it is running (for example, audiences, attributes, variation pausing, and traffic distribution). Otherwise, the Python SDK is stateless and rely on deterministic bucketing to return consistent assignments. See [How bucketing works](doc:how-bucketing-works) for more information.
[block:api-header]
{
  "title": "Implement a service"
}
[/block]
Refer to the code samples below to provide your own User Profile Service. It should expose two functions with the following signatures:

* `lookup`: Takes a user ID string and returns a user profile matching the schema below.
* `save`: Takes a user profile and persists it.

If you want to use the User Profile Service purely for tracking purposes and not sticky bucketing, you can implement only the `save` method (always return `nil` from `lookup`).
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import user_profile\nfrom optimizely import optimizely\n\nclass MyUserProfileService(user_profile.UserProfileService):\n    def lookup(self, user_id):\n        pass\n        # Retrieve and return user profile\n\n    def save(self, user_profile):\n        pass\n        # Save user profile\n\noptimizely_client = optimizely.Optimizely(datafile, user_profile_service=MyUserProfileService())\n\n",
      "language": "python"
    }
  ]
}
[/block]
The code example below shows the JSON schema for the user profile object.

Use `experiment_bucket_map` to override the default bucketing behavior and define an alternate experiment variation for a given user. For each experiment that you want to override, add an object to the map. Use the experiment ID as the key and include a `variation_id` property that specifies the desired variation. If there isn't an entry for an experiment, then the default bucketing behavior persists.

In the example below, `^[a-zA-Z0-9]+$` is the experiment ID.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"title\": \"UserProfile\",\n  \"type\": \"object\",\n  \"properties\": {\n    \"user_id\": {\"type\": \"string\"},\n    \"experiment_bucket_map\": {\"type\": \"object\",\n                              \"patternProperties\": {\n                                 \"^[a-zA-Z0-9]+$\": {\"type\": \"object\",\n                                                    \"properties\": {\"variation_id\": {\"type\":\"string\"}},\n                                                    \"required\": [\"variation_id\"]}\n                               }\n                             }\n  },\n  \"required\": [\"user_id\", \"experiment_bucket_map\"]\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
The Python SDK uses the User Profile Service you provide to override Optimizely's default bucketing behavior in cases when an experiment assignment has been saved.

When implementing your own User Profile Service, we recommend loading the user profiles into the User Profile Service on initialization and avoiding performing expensive, blocking lookups on the lookup function to minimize the performance impact of incorporating the service.

When implementing in a multi-server or stateless environment, we suggest using this interface with a backend like [Cassandra](http://cassandra.apache.org/) or [Redis](https://redis.io/). You can decide how long you want to keep your sticky bucketing around by configuring these services.
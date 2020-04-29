---
title: "Implement a user profile service"
slug: "implement-a-user-profile-service-objective-c"
hidden: true
createdAt: "2019-09-12T14:17:17.895Z"
updatedAt: "2019-09-12T15:59:58.146Z"
---
Use a **User Profile Service** to persist information about your users and ensure variation assignments are sticky. For example, if you are working on a backend website, you can create an implementation that reads and saves user profiles from a Redis or memcached store. 

The Objective-C SDK defaults to a User Profile Service that stores this state directly on the device. See the [Objective-C SDK User Profile Service](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKUserProfileService/OptimizelySDKUserProfileService/OPTLYUserProfileService.m).

Use `manager.userProfileService.lookup` to read a customer’s user profile.
[block:api-header]
{
  "title": "No op in Objective-C"
}
[/block]
You can overwrite the User Profile Service by setting the User Profile Service to `OPTLYUserProfileServiceNoOp.init()​`. Here is an example:
[block:code]
{
  "codes": [
    {
      "code": "let optimizelyManager = OPTLYManager.init { \n    (builder) in builder !.projectId =\n    \"12345678\" builder ?.userProfileService =\n    OPTLYUserProfileServiceNoOp.init () \n}\n \noptimizelyManager ?.initialize (callback : { \n\t(err, client) in\t// activate an experiment let variation = client?.activate(\"Test\", userId: \"user_test_1\") NSLog((variation?.variationKey)!)\n}\n                              \n                              ",
      "language": "objectivec"
    }
  ]
}
[/block]
The bucketing will no longer be sticky for returning visitors: new traffic allocation changes will be applied for all visitors. The default user profile in iOS stores the bucketing decision for every visitor, so if the traffic allocation changes, this change is not applied to visitors who already have bucketing in storage.
[block:api-header]
{
  "title": "Implement a User Profile Service"
}
[/block]
Your User Profile Service should expose two functions with the following signatures:

* `lookup`: Takes a user ID string and returns a user profile matching the schema below.
* `save`: Takes a user profile and persists it.

If you want to use the User Profile Service purely for tracking purposes and not sticky bucketing, you can implement only the `save` method (always return `nil` from `lookup`).

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
The SDK uses the User Profile Service you provide to override Optimizely's default bucketing behavior in cases when an experiment assignment has been saved.

The User Profile Service will persist variation assignments across app updates. However, the User Profile Service will not persist variation assignments across app re-installs.

When implementing your own User Profile Service, we recommend loading the user profiles into the User Profile Service on initialization and avoiding performing expensive, blocking lookups on the lookup function to minimize the performance impact of incorporating the service.

When implementing in a multi-server or stateless environment, we suggest using this interface with a backend like [Cassandra](http://cassandra.apache.org/) or [Redis](https://redis.io/). You can decide how long you want to keep your sticky bucketing around by configuring these services.
---
title: "Implement a user profile service"
slug: "implement-a-user-profile-service-javascript"
hidden: true
createdAt: "2019-09-12T14:17:33.661Z"
updatedAt: "2019-09-12T21:28:17.809Z"
---
Use a **User Profile Service** to persist information about your users and ensure variation assignments are sticky. For example, if you are working on a backend website, you can create an implementation that reads and saves user profiles from a Redis or memcached store. 

In the JavaScript SDK, there is no default implementation. Implementing a User Profile Service is optional and is only necessary if you want to keep variation assignments sticky even when experiment conditions are changed while it is running (for example, audiences, attributes, variation pausing, and traffic distribution). Otherwise, the JavaScript SDK is stateless and rely on deterministic bucketing to return consistent assignments. See [How bucketing works](doc:how-bucketing-works) for more information.
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
      "code": "// Sample user profile service implementation\nconst userProfileService = {\n  lookup: userId => {\n    // Perform user profile lookup\n  },\n  save: userProfileMap => {\n    // Persist user profile\n  },\n};\n\nvar optimizelyClient = optimizely.createInstance({\n  datafile,\n  userProfileService,\n});\n\n",
      "language": "javascript"
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
The SDK uses the User Profile Service you provide to override Optimizely's default bucketing behavior in cases when an experiment assignment has been saved.

For both Swift and Android apps, the User Profile Service will persist variation assignments across app updates. However, the User Profile Service will not persist variation assignments across app re-installs.

When implementing your own User Profile Service, we recommend loading the user profiles into the User Profile Service on initialization and avoiding performing expensive, blocking lookups on the lookup function to minimize the performance impact of incorporating the service.

When implementing in a multi-server or stateless environment, we suggest using this interface with a backend like [Cassandra](http://cassandra.apache.org/) or [Redis](https://redis.io/). You can decide how long you want to keep your sticky bucketing around by configuring these services.
[block:api-header]
{
  "title": "Implement an asynchronous service in JavaScript SDK 3.0"
}
[/block]
In the JavaScript SDK 3.0, you can implement `attributes.$opt_experiment_bucket_map` to perform asynchronous lookups of users' previous variations. The SDK handles `attributes.$opt_experiment_bucket_map` the same way it would `userProfileService.lookup`, and this allows you to do an asynchronous lookup of the experiment bucket map before passing it to the [Activate](doc:activate-javascript) method. 
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "- `attributes.$opt_experiment_bucket_map` will always take precedence over an implemented `userProfileService.lookup`.\n- Because the Javascript SDK is stateless, you must use these attributes anywhere that you call the [Activate](doc:activate-javascript), [Get Variation](doc:get-variation-javascript), or [Track](doc:track-javascript) methods."
}
[/block]
The example below shows how to implement consistent bucketing via attributes.
[block:code]
{
  "codes": [
    {
      "code": "const userId = 'user1'\n// This would come from a DB call\nconst experimentBucketMap = {\n  '123': { // experimentId\n    'variation_id': '456', // the variationId\n  }\n}\nconst attributes = {\n  '$opt_experiment_bucket_map': experimentBucketMap\n}\n/* The user will always get bucketed into variationid='456' for experiment id='123 */\nconst result = client.activate(\"my-experiment\", userId, attributes)\n\n",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
You can use the asynchronous service example below to try this functionality in a test environment. If you implement this example in a production environment, be sure to modify `UserProfileDB` to the correct database.
[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk')\n\n// This is here only as an example; in a production environment this would be redis or some distributed database\nclass UserProfileDB {\n  constructor() {\n    /* Example structure\n     * {\n     *   user1: {\n     *     user_id: 'user1',\n     *     experiment_bucket_map: {\n     *       '12095834311': { // experimentId\n     *         variation_id: '12117244349' // variationId\n     *       }\n     *     }\n     *   }\n     * }\n     */\n    this.db = {}\n  }\n\n  async save(user_id, experiment_bucket_map) {\n    return new Promise((resolve, reject) => {\n      // Use setTimeout to simulate async\n      setTimeout(() => {\n        this.db[user_id] = { user_id, experiment_bucket_map }\n        resolve()\n      }, 50)\n    })\n  }\n\n  async lookup(userId) {\n    return new Promise((resolve, reject) => {\n      // Use setTimeout to simulate async\n      setTimeout(() => {\n        let result\n        if (this.db[userId] && this.db[userId].experiment_bucket_map) {\n          result = this.db[userId].experiment_bucket_map\n        }\n        resolve(result)\n      }, 50)\n    })\n  }\n}\n\nconst userDb = new UserProfileDB()\n\nconst userProfileService = {\n  lookup(userId) {\n    // Lookup must be synchronous, so in our case we will not implement this function\n  },\n  save(userProfileMap) {\n    const { user_id, experiment_bucket_map } = userProfileMap\n    userDb.save(user_id, experiment_bucket_map)\n  }\n}\n\nconst client = optimizely.createInstance({\n  datafile,\n  userProfileService,\n})\n\nconst userId = 'user1'\n// Lookup the users experiment_bucket_map\nconst experimentBucketMap = await userDb.lookup(userId) || {}\nconst attributes = {\n  $opt_experiment_bucket_map: experimentBucketMap\n}\n\nconst result = client.activate(\"exp1\", userId, attributes)\nconsole.log('got variation', result) \n\n",
      "language": "javascript"
    }
  ]
}
[/block]
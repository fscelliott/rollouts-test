---
title: "Implement a user profile service"
slug: "implement-a-user-profile-service-react"
hidden: false
createdAt: "2019-09-12T14:17:27.377Z"
updatedAt: "2019-09-25T20:40:11.828Z"
---
Use a **User Profile Service** to persist information about your users and ensure variation assignments are sticky. Sticky implies that once a user gets a particular variation, their assignment won't change.

In the React SDK, there is no default implementation. Implementing a User Profile Service is optional and is only necessary if you want to keep variation assignments sticky even when experiment conditions are changed while it is running (for example, audiences, attributes, variation pausing, and traffic distribution). Otherwise, the React SDK is stateless and relies on deterministic bucketing to return consistent assignments. See [How bucketing works](doc:how-bucketing-works) for more information.
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
      "code": "import { createInstance } from '@optimizely/react-sdk';\n\n// Sample user profile service implementation\nconst userProfileService = {\n  lookup: userId => {\n    // Perform user profile lookup\n  },\n  save: userProfileMap => {\n    // Persist user profile\n  },\n};\n\nconst optimizelyClient = createInstance({\n  datafile: window.datafile, // assuming you have a datafile at window.datafile\n  userProfileService, // Passing your userProfileService created above\n});\n\n",
      "language": "javascript",
      "name": "React"
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
The React SDK uses the User Profile Service you provide to override Optimizely's default bucketing behavior in cases when an experiment assignment has been saved.

When implementing your own User Profile Service, we recommend loading the user profiles into the User Profile Service on initialization and avoiding performing expensive, blocking lookups on the lookup function to minimize the performance impact of incorporating the service.
[block:api-header]
{
  "title": "Implement an asynchronous service"
}
[/block]
You can implement `attributes.$opt_experiment_bucket_map` to perform asynchronous lookups of users' previous variations. The SDK handles `attributes.$opt_experiment_bucket_map` the same way it would `userProfileService.lookup`, and this allows you to do an asynchronous lookup of the experiment bucket map before passing it to the [Activate](doc:activate-react) method. 
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "- `attributes.$opt_experiment_bucket_map` will always take precedence over an implemented `userProfileService.lookup`.\n- Because the React SDK is stateless, you must use these attributes anywhere that you call the [Activate](doc:activate-react), [Get Variation](doc:get-variation-react), or [Track](doc:track-react) methods."
}
[/block]
The example below shows how to implement consistent bucketing via attributes.
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n} from '@optimizely/react-sdk'\n\nconst optimizelyClient = createInstance({\n  datafile: window.datafile, // assuming you have a datafile at window.datafile\n});\n\n// In practice, this could come from a DB call\nconst experimentBucketMap = {\n  123: { // ID of experiment\n    variation_id: '456', // ID of variation to force for this experiment\n  }\n}\n\nconst user = {\n  id: ‘myuser123’,\n  attributes: {\n    // By passing this $opt_experiment_bucket_map, we force that the user\n    // will always get bucketed into variationid='456' for experiment id='123'\n    '$opt_experiment_bucket_map': experimentBucketMap,\n  },\n};\n\nfunction App() {\n  return (\n    <OptimizelyProvider\n      optimizely={optimizely}\n      user={user}\n    >\n    {/* … your application components here … */}\n    </OptimizelyProvider>\n  </App>\n}",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
You can use the asynchronous service example below to try this functionality in a test environment. If you implement this example in a production environment, be sure to modify `UserProfileDB` to a real database.
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n} from '@optimizely/react-sdk'\n\n// This is here only as an example; in a production environment this could access a real datastore\nclass UserProfileDB {\n  constructor() {\n    /* Example structure\n     * {\n     *   user1: {\n     *     user_id: 'user1',\n     *     experiment_bucket_map: {\n     *       '12095834311': { // experimentId\n     *         variation_id: '12117244349' // variationId\n     *       }\n     *     }\n     *   }\n     * }\n     */\n    this.db = {}\n  }\n\n  save(user_id, experiment_bucket_map) {\n    return new Promise((resolve, reject) => {\n      setTimeout(() => {\n        this.db[user_id] = { user_id, experiment_bucket_map }\n        resolve()\n      }, 50)\n    })\n  }\n\n  lookup(userId) {\n    return new Promise((resolve, reject) => {\n      setTimeout(() => {\n        let result\n        if (this.db[userId] && this.db[userId].experiment_bucket_map) {\n          result = this.db[userId].experiment_bucket_map\n        }\n        resolve(result)\n      }, 50)\n    })\n  }\n}\n\nconst userDb = new UserProfileDB()\n\nconst userProfileService = {\n  lookup(userId) {\n    // In our case we will not implement this function here. We will look up the attributes for the user below.\n  },\n  save(userProfileMap) {\n    const { user_id, experiment_bucket_map } = userProfileMap\n    userDb.save(user_id, experiment_bucket_map)\n  }\n}\n\nconst client = createInstance({\n  datafile: window.datafile, // assuming you have a datafile at window.datafile\n  userProfileService,\n})\n\n// React SDK supports passing a Promise as user, for async user lookups like this\nconst user = userDb.lookup(userId).then((experimentBucketMap = {}) => {\n  return {\n    id: 'user1',\n    attributes: {\n      $opt_experiment_bucket_map: experimentBucketMap\n    },\n  }\n})\n\n// The provider will use the given user and optimizely instance.\n// The provided experiment bucket map will force any specified variation\n// assignments from userDb.\n// The provided user profile service will save any new variation assignments to\n//userDb.\nfunction App() {\n  return (\n    <OptimizelyProvider\n      optimizely={optimizely}\n      user={user}\n    >\n    {/* … your application components here … */}\n    </OptimizelyProvider>\n  </App>\n}\n",
      "language": "javascript"
    }
  ]
}
[/block]
---
title: "Is Feature Enabled"
slug: "is-feature-enabled-swift"
hidden: false
createdAt: "2019-09-12T17:00:42.124Z"
updatedAt: "2019-09-13T13:07:21.347Z"
---
Determines whether a feature is enabled for a given user. The purpose of this method is to allow you to separate the process of developing and deploying features from the decision to turn on a feature. Build your feature and deploy it to your application behind this flag, then turn the feature on or off for specific users.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.1.0
[block:api-header]
{
  "title": "Description"
}
[/block]
This method traverses the client's datafile and checks the feature flag for the feature key that you specify.

This method evaluates the feature rollout for a user. The method checks whether the rollout is enabled, whether the user qualifies for the audience targeting, and then randomly assigns either `on` or `off` based on the appropriate traffic allocation. If the feature rollout is on for a qualified user, the method returns `True`. 
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Swift.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "0-0": "**featureKey**\n*required*",
    "0-1": "String",
    "1-0": "**userId**\n*required*",
    "1-1": "String",
    "2-0": "**attributes**\n*optional*",
    "2-1": "Dictionary<String, Any>",
    "h-2": "Description",
    "0-2": "The key of the feature to check. \n\nThe feature key is defined from the Features dashboard.",
    "1-2": "The ID of the user to check.",
    "2-2": "A map of custom key-value string pairs specifying attributes for the user that are used for audience targeting."
  },
  "cols": 3,
  "rows": 3
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
YES if the feature is enabled for the user. Otherwise, false.
[block:api-header]
{
  "title": "Examples"
}
[/block]
This section shows a simple example of how you can use the `IsFeatureEnabled` method.
[block:code]
{
  "codes": [
    {
      "code": "// Evaluate a feature flag and a variable\nlet enabled = optimizely.isFeatureEnabled(featureKey: \"price_filter\", userId: userId)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nBOOL enabled = [optimizely isFeatureEnabledWithFeatureKey:@\"my_feature_key\"\n                     \t\t\t\t\t\tuserId:@\"user_123\"\n                     \t\t\t\t\t\tattributes:attributes];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Swift is [OptimizelyClient.swift](https://github.com/optimizely/swift-sdk/blob/master/OptimizelySDK/Optimizely/OptimizelyClient.swift).
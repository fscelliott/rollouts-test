---
title: "Get Forced Variation"
slug: "get-forced-variation-swift"
hidden: false
createdAt: "2019-09-12T17:00:25.636Z"
updatedAt: "2020-02-20T19:08:31.506Z"
---
Returns the forced variation set by [Set Forced Variation](doc:set-forced-variation-swift), or `null` if no variation was forced.

A user can be forced into a variation for a given experiment for the lifetime of the Optimizely client. This method gets the variation that the user has been forced into. The forced variation value is runtime only and does not persist across application launches.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.0, v3.1
[block:api-header]
{
  "title": "Description"
}
[/block]
Forced bucketing variations take precedence over whitelisted variations, variations saved in a User Profile Service (if one exists), and the normal bucketed variation. Variations are overwritten when [Set Forced Variation](doc:set-forced-variation-swift) is invoked.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "A forced variation only persists for the lifetime of an Optimizely client."
}
[/block]

[block:api-header]
{
  "title": "Parameters"
}
[/block]
This table lists the required and optional parameters for the Swift SDK.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**experimentKey**\n*required*",
    "0-1": "string",
    "1-0": "**userId**\n*required*",
    "1-1": "string",
    "0-2": "The key of the experiment to retrieve the forced variation.",
    "1-2": "The ID of the user in the forced variation."
  },
  "cols": 3,
  "rows": 2
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
It returns a forced variation if it exists, otherwise returns nil.
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "let variationKey = optimizely.getForcedVariation(experimentKey: \"my_experiment_key\", \n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tuserId: \"user_123\")\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "NSString *variationKey = [optimizely getForcedVariationWithExperimentKey:@\"my_experiment_key\"\n                          userId:@\"user_123\"];\n\n",
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
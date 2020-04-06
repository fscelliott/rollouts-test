---
title: "Set Forced Variation"
slug: "set-forced-variation-swift"
hidden: false
createdAt: "2019-09-12T17:03:31.482Z"
updatedAt: "2019-09-13T13:07:49.317Z"
---
Forces a user into a variation for a given experiment for the lifetime of the Optimizely client.

The purpose of this method is to force a user into a specific variation or personalized experience for a given experiment. The forced variation value doesn't persist across application launches.
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
Forces a user into a variation for a given experiment for the lifetime of the Optimizely client. Any future calls to [Activate](doc:activate-swift), [Is Feature Enabled](doc:is-feature-enabled-swift), [Get Feature Variable](doc:get-feature-variable-swift), and [Track](doc:track-swift) for the given user ID returns the forced variation.

Forced bucketing variations take precedence over whitelisted variations, variations saved in a User Profile Service (if one exists), and the normal bucketed variation. Impression and conversion events are still tracked when forced bucketing is enabled.

Variations are overwritten with each set method call. To clear the forced variations so that the normal bucketing flow can occur, pass null as the variation key parameter. To get the variation that has been forced, use [Get Forced Variation](doc:get-forced-variation-swift).

This call will fail and return false if the experiment key is not in the project file or if the variation key is not in the experiment.

You can also use Set Forced Variation for [feature tests](doc:run-feature-tests).
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
    "0-0": "**experiment_key**\n*required*",
    "0-1": "string",
    "1-0": "**user_id**\n*required*",
    "1-1": "string",
    "0-2": "The key of the experiment to set with the forced variation.",
    "1-2": "The ID of the user to force into the variation.",
    "2-2": "The key of the forced variation. Set the value to `null` to clear the existing experiment-to-variation mapping.",
    "2-0": "**variation_key**\n*optional*",
    "2-1": "string"
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
A Boolean value that indicates if the set completed successfully.
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "optimizely.setForcedVariation(experimentKey: \"my_experiment_key\",\n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tuserId: \"user_123\",\n    \t\t\t\t\t\t\t\t\t\t\t\t\tvariationKey: \"some_variation_key\")\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "[optimizely setForcedVariationWithExperimentKey: @\"my_experiment_key\"\n               \t\t\t\t\t\tuserId:@\"user_123\"\n               \t\t\t\t\t\tvariationKey:@\"some_variation_key\"];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Side effects"
}
[/block]
In the receiving client instance, sets the forced variation for the specified user in the specified experiment. This forced variation is used instead of the variation that Optimizely would normally determine for that user and experiment.
[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Swift is [OptimizelyClient.swift](https://github.com/optimizely/swift-sdk/blob/master/OptimizelySDK/Optimizely/OptimizelyClient.swift).
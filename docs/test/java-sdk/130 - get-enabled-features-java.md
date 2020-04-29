---
title: "Get Enabled Features"
slug: "get-enabled-features-java"
hidden: false
createdAt: "2019-08-21T21:29:14.601Z"
updatedAt: "2019-09-13T11:54:25.238Z"
---
Retrieves a list of features that are enabled for the user. Invoking this method is equivalent to running [Is Feature Enabled](doc:is-feature-enabled-java) for each feature in the datafile sequentially.

This method takes into account the user `attributes` passed in, to determine if the user is part of the audience that qualifies for the experiment.  
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.0
[block:api-header]
{
  "title": "Description"
}
[/block]
This method iterates through all feature flags and for each feature flag invokes [Is Feature Enabled](doc:is-feature-enabled-java). If a feature is enabled, this method adds the feature’s key to the return list.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Java.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**userId**\n*required*",
    "0-1": "string",
    "1-0": "**attributes**\n*optional*",
    "1-1": "map",
    "0-2": "The ID of the user to check. For more information, see [Handle user IDs](doc:handle-user-ids).",
    "1-2": "A map of custom key-value string pairs specifying attributes for the user that are used for [audience targeting](doc:audiences). Non-string values are only supported in the 3.0 SDK and above."
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
A list of keys corresponding to the features that are enabled for the user, or an empty list if no features could be found for the specified user.
[block:api-header]
{
  "title": "Examples"
}
[/block]
This section shows a simple example of how you can use the method.

The Java example shows 3.0-specific functionality of passing in [non-string attributes](doc:target-audiences). 
[block:code]
{
  "codes": [
    {
      "code": "Map<String, Object> attributes = new HashMap<>();\nattributes.put(\"DEVICE\", \"iPhone\");\nattributes.put(\"LIFETIME\", 24738388);\nattributes.put(\"IS_LOGGED_IN\", true);\n\nArrayList<String> featureFlags = (ArrayList<String>) spyOptimizely.getEnabledFeatures(genericUserId, attributes);\n\n",
      "language": "java"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Java is [Optimizely.java](https://github.com/optimizely/java-sdk/blob/master/core-api/src/main/java/com/optimizely/ab/Optimizely.java).
---
title: "Get Enabled Features"
slug: "get-enabled-features-javascript"
hidden: false
createdAt: "2019-08-21T21:35:15.808Z"
updatedAt: "2019-09-13T11:54:35.188Z"
---
Retrieves a list of features that are enabled for the user. Invoking this method is equivalent to running [Is Feature Enabled](doc:is-feature-enabled-javascript) for each feature in the datafile sequentially.

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
This method iterates through all feature flags and for each feature flag invokes [Is Feature Enabled](doc:is-feature-enabled-javascript). If a feature is enabled, this method adds the feature’s key to the return list.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in JavaScript.
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

The JavaScript example shows 3.0-specific functionality of passing in [non-string attributes](doc:target-audiences). 
[block:code]
{
  "codes": [
    {
      "code": "var result = optlyInstance.getEnabledFeatures(\n  'user1',\n  {\n    device: 'iPhone',\n    lifetime: 24738388,\n    is_logged_in: true,\n  },\n);\n  \n  ",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for JavaScript is [index.js](https://github.com/optimizely/javascript-sdk/blob/master/packages/optimizely-sdk/lib/optimizely/index.js).
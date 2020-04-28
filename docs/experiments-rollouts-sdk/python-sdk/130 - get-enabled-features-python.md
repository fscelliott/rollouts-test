---
title: "Get Enabled Features"
slug: "get-enabled-features-python"
hidden: false
createdAt: "2019-08-21T21:50:29.910Z"
updatedAt: "2019-09-13T11:55:09.846Z"
---
Retrieves a list of features that are enabled for the user. Invoking this method is equivalent to running [Is Feature Enabled](doc:is-feature-enabled-python) for each feature in the datafile sequentially.

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
This method iterates through all feature flags and for each feature flag invokes [Is Feature Enabled](doc:is-feature-enabled-python). If a feature is enabled, this method adds the feature’s key to the return list.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Python.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**user_id**\n*required*",
    "0-1": "string",
    "1-0": "**attributes**\n*optional*",
    "1-1": "dict",
    "0-2": "The ID of the user to check for access to features. For more information, see [Handle user IDs](doc:handle-user-ids).",
    "1-2": "A dict of custom key-value pairs specifying attributes for the user that are used for [audience targeting](doc:target-audiences). Non-string values are only supported in the 3.0 SDK and above.",
    "h-3": "Description",
    "0-3": "",
    "1-3": ""
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
A list of the keys of the features that are enabled for the user.
[block:api-header]
{
  "title": "Examples"
}
[/block]
This section shows a simple example of how you can use the method.
[block:code]
{
  "codes": [
    {
      "code": "enabled_features = optimizely_instance.get_enabled_features(user_id, attributes)\n\n",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Python is [optimizely.py](https://github.com/optimizely/python-sdk/blob/master/optimizely/optimizely.py).
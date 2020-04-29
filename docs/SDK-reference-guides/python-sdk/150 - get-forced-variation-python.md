---
title: "Get Forced Variation"
slug: "get-forced-variation-python"
hidden: true
createdAt: "2020-04-27T16:15:19.158Z"
updatedAt: "2020-04-28T15:23:03.928Z"
---
Returns the forced variation set by [Set Forced Variation](doc:set-forced-variation-python), or `null` if no variation was forced.

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
Forced bucketing variations take precedence over whitelisted variations, variations saved in a User Profile Service (if one exists), and the normal bucketed variation. Variations are overwritten when [Set Forced Variation](doc:set-forced-variation-python) is invoked.
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
This table lists the required and optional parameters for the Python SDK.
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
The forced variation key. None if no forced variation key.
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "variation_key = optimizely_client.get_forced_variation('my_experiment_key', 'user_123');\n\n",
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
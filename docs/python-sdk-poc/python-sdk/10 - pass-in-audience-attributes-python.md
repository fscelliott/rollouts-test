---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-python"
hidden: false
createdAt: "2019-08-21T21:49:54.404Z"
updatedAt: "2019-08-21T21:50:02.394Z"
---
You can pass strings, Booleans, integers, floating point values, and None as user attribute values. The example below shows how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "attributes = {\n  'device': 'iPhone',\n  'lifetime': 24738388,\n  'is_logged_in': True,\n}\n\nenabled = optimizely_client.is_feature_enabled('new_feature', 'user123', attributes)\n",
      "language": "python"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-python) will include warnings when this occurs."
}
[/block]
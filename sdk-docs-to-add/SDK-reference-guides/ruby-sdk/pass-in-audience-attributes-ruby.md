---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-ruby"
hidden: false
createdAt: "2019-08-21T21:53:41.194Z"
updatedAt: "2019-08-21T21:53:46.362Z"
---
You can pass strings, numbers, Booleans, and nil as user attribute values. The examples below show how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "attributes = {\n  'DEVICE' => 'iPhone',\n  'AD_SOURCE' => 'my_campaign'\n  'IS_LOGGED_IN' => true\n}\n\nenabled = optimizely_client.is_feature_enabled('new_feature', 'user123', attributes)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-ruby) will include warnings when this occurs."
}
[/block]
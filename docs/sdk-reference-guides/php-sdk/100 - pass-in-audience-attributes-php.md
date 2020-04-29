---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-php"
hidden: false
createdAt: "2019-08-21T21:43:58.626Z"
updatedAt: "2019-08-21T21:44:04.814Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The examples below show how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "$attributes = [\n    'device' => 'iphone',\n    'ad_source' => 'my_campaign'\n];\n\n$enabled = $optimizelyClient->isFeatureEnabled('new_feature', 'user123', $attributes);\n\n",
      "language": "php"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-php) will include warnings when this occurs."
}
[/block]
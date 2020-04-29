---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-java"
hidden: false
createdAt: "2019-08-21T21:28:32.617Z"
updatedAt: "2019-08-21T21:28:58.519Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The example below shows how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "Map<String, Object> attributes = new HashMap<>();\nattributes.put(\"device\", \"iPhone\");\nattributes.put(\"lifetime\", 24738388);\nattributes.put(\"is_logged_in\", true);\n\nBoolean enabled = optimizelyClient.isFeatureEnabled(\"new_feature\", \"user123\", attributes);\n\n",
      "language": "java"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-java) will include warnings when this occurs."
}
[/block]
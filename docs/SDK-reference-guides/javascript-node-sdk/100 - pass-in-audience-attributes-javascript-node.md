---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-javascript-node"
hidden: false
createdAt: "2019-08-21T21:39:15.269Z"
updatedAt: "2019-08-21T21:39:17.979Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The examples below show how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "const attributes = {\n  device: 'iPhone',\n  lifetime: 24738388,\n  is_logged_in: true,\n};\n\nconst enabled = optimizelyClientInstance.isFeatureEnabled(\n  \"new_feature\",\n  \"user123\",\n  attributes\n);\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-javascript-node) will include warnings when this occurs."
}
[/block]
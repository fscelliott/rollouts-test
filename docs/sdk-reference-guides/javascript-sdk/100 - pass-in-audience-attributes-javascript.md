---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-javascript"
hidden: false
createdAt: "2019-08-21T21:34:58.831Z"
updatedAt: "2019-08-21T21:35:05.812Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The examples below show how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "const attributes = {\n  device: 'iPhone',\n  lifetime: 24738388,\n  is_logged_in: true,\n};\n\nconst enabled = optimizelyClientInstance.isFeatureEnabled('new_feature', 'user123', attributes);\n\n",
      "language": "javascript"
    },
    {
      "code": "<OptimizelyProvider\n  optimizely={optimizely}\n  userId={window.userId}\n  userAttributes={{\n    device: 'iPhone',\n    lifetime: 24738388,\n    is_logged_in: true,\n  }}\n>\n</OptimizelyProvider>\n\n",
      "language": "html",
      "name": "React"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-javascript) will include warnings when this occurs."
}
[/block]
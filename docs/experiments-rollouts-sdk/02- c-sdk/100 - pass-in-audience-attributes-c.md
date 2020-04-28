---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-c"
hidden: false
createdAt: "2019-08-21T21:10:39.230Z"
updatedAt: "2019-09-27T21:56:04.776Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The example below shows how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "UserAttributes attributes = new UserAttributes\n{\n  { \"DEVICE\", \"iPhone\" },\n  { \"AD_SOURCE\", \"my_campaign\" }\n};\n\nbool enabled = OptimizelyClient.IsFeatureEnabled(\"new_feature\", \"user123\", attributes);\n\n",
      "language": "csharp"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-c) will include warnings when this occurs."
}
[/block]
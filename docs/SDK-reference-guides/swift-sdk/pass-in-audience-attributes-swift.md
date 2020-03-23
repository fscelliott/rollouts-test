---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes-swift"
hidden: false
createdAt: "2019-09-12T16:59:53.515Z"
updatedAt: "2019-09-12T17:02:56.670Z"
---
You can pass strings, numbers, Booleans, and null as user attribute values. The examples below show how to pass in attributes.
[block:code]
{
  "codes": [
    {
      "code": "let attributes: [String: Any] = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\n// Conditionally activate an A/B test\nlet variationKey = try? optimizely.activate(experimentKey: experimentKey, userId: userId, attributes: attributes)\n// Track a conversion event\ntry? optimizely.track(eventKey: eventKey, userId:userId, attributes:attributes)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\n// Conditionally activate an A/B test\nNSString *variationKey = [optimizely activateWithExperimentKey:experimentKey,\n                                                userId:userId,\n                                            attributes:attributes,\n                            \t\t\t\t\t\t\t\t\t\t error:nil];\n// Track a conversion event\n[optimizely track:eventKey\n                 userId:userId\n             attributes:attributes,\n\t\t\t\t\t\t\t\t\terror:nil];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-swift) will include warnings when this occurs."
}
[/block]
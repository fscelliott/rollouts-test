---
title: "Include event tags"
slug: "include-event-tags"
hidden: false
createdAt: "2019-09-11T11:47:40.633Z"
updatedAt: "2020-02-21T19:24:06.473Z"
---
*Event tags* are contextual metadata about conversion events that you track.

Use event tags to attach key-value data to events. For example, for a product purchase event, you may want to attach a product SKU, product category, order ID, and purchase amount. Event tags can be strings, integers, floating point numbers, or boolean values.

You can include event tags as an optional argument in Track; see the example below.
[block:code]
{
  "codes": [
    {
      "code": "String eventKey = \"my_conversion\";\nString userId = \"user123\";\n\nMap<String, String> attributes = new HashMap<String, String>();\nattributes.put(\"DEVICE\", \"iPhone\");\nattributes.put(\"AD_SOURCE\", \"my_campaign\");\n\nMap<String, Object> eventTags = new HashMap<String, Object>();\neventTags.put(\"purchasePrice\", 64.32f);\neventTags.put(\"category\", \"shoes\");\n\n// Reserved \"revenue\" tag\neventTags.put(\"revenue\", 6432);\n\n// Reserved \"value\" tag\neventTags.put(\"value\", 4);\n\nOptimizely optimizelyClient = optimizelyManager.getOptimizely();\n\n// Track event with user attributes and event tags\noptimizelyClient.track(eventKey, userId, attributes, eventTags);\n\n",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "using OptimizelySDK.Entity;\n\nvar eventKey = \"my_conversion\";\nvar userId = \"user123\";\n\nUserAttributes attributes = new UserAttributes\n{\n    { \"DEVICE\", \"iPhone\" },\n    { \"AD_SOURCE\", \"my_campaign\" }\n};\n\nEventTags tags = new EventTags\n{\n    { \"purchasePrice\", 64.32 },\n    { \"category\", \"shoes\" },\n    { \"revenue\", 6432 },  // Reserved \"revenue\" tag\n    { \"value\", 4 }  // Reserved \"value\" tag\n};\n\n// Track event with user attributes and event tags\nOptimizelyClient.Track(eventKey, userId, attributes, tags);\n\n",
      "language": "csharp"
    },
    {
      "code": "String eventKey = \"my_conversion\";\nString userId = \"user123\";\n\nMap<String, String> attributes = new HashMap<String, String>();\nattributes.put(\"DEVICE\", \"iPhone\");\nattributes.put(\"AD_SOURCE\", \"my_campaign\");\n\nMap<String, Object> eventTags = new HashMap<String, Object>();\neventTags.put(\"purchasePrice\", 64.32f);\neventTags.put(\"category\", \"shoes\");\n\n// Reserved \"revenue\" tag\neventTags.put(\"revenue\", 6432);\n\n// Reserved \"value\" tag\neventTags.put(\"value\", 4);\n\n// Track event with user attributes and event tags\noptimizelyClient.track(eventKey, userId, attributes, eventTags);\n\n",
      "language": "java"
    },
    {
      "code": "const eventKey = 'my_conversion';\nconst userId = 'user123';\n\nconst attributes = {\n  DEVICE: 'iPhone',\n  AD_SOURCE: 'my_campaign',\n};\n\nconst eventTags = {\n  category: 'shoes',\n  purchasePrice: 64.32,\n  revenue: 6432, // reserved \"revenue\" tag\n  value: 4, // reserved \"value\" tag\n};\n\n// Track event with user attributes and event tags\noptimizelyClient.track(eventKey, userId, attributes, eventTags);\n\n",
      "language": "javascript"
    },
    {
      "code": "NSDictionary *attributes = @{@\"device\" : @\"iPhone\", @\"ad_source\" : @\"my_campaign\"};\n\nNSDictionary *eventTags = @{\n  @\"purchasePrice\" : @64.32,\n  @\"category\" : @\"shoes\",\n  @\"revenue\": @6432  // reserved \"revenue\" tag\n};\n\n// Track event with user attributes and event tags\n[optimizely track:@\"my_conversion\"\n           userId:@\"user123\"\n       attributes:attributes\n       eventTags:eventTags];\n\n",
      "language": "objectivec"
    },
    {
      "code": "$eventKey = 'my_conversion';\n$userId = 'user123';\n\n$attributes = [\n    'device' => 'iphone',\n    'ad_source' => 'my_campaign'\n];\n\n$eventTags = [\n    'category' => 'shoes',\n    'purchasePrice' => 64.32,\n    'revenue' => 6432, // reserved \"revenue\" tag\n    'value' => 4 // reserved \"value\" tag\n];\n\n// Track event with user attributes and event tags\n$optimizelyClient->track($eventKey, $userId, $attributes, $eventTags);\n\n// Track event with event tags and without user attributes\n$optimizelyClient->track($eventKey, $userId, null, $eventTags);\n\n",
      "language": "php"
    },
    {
      "code": "event_key = 'my_conversion'\nuser_id = 'user123'\n\nattributes = {\n  'device': 'iPhone',\n  'adSource': 'my_campaign'\n}\n\nevent_tags = {\n  'category': 'shoes',\n  'purchasePrice': 64.32,\n  'revenue': 6432,  # reserved \"revenue\" tag\n  'value': 4 # reserved \"value\" tag\n}\n\n# Track event with user attributes and event tags\noptimizely_client.track(event_key, user_id, attributes, event_tags)\n\n# Track event with event tags and without user attributes\noptimizely_client.track(event_key, user_id, event_tags=event_tags)\n\n",
      "language": "python"
    },
    {
      "code": "event_key = 'my_conversion'\nuser_id = 'user123'\n\nattributes = {\n  'device' => 'iPhone',\n  'adSource' => 'my_campaign'\n}\n\nevent_tags = {\n  'category' => 'shoes',\n  'purchasePrice' => 64.32,\n  'revenue' => 6432,  # reserved \"revenue\" tag\n  'value' => 4 # reserved \"value\" tag\n}\n\n# Track event with user attributes and event tags\noptimizely_client.track(event_key, user_id, attributes, event_tags)\n\n# Track event with event tags and without user attributes\noptimizely_client.track(event_key, user_id, nil, event_tags)\n\n",
      "language": "ruby"
    },
    {
      "code": "let attributes = [\"device\" : \"iPhone\", \"ad_source\" : \"my_campaign\"]\n\nvar eventTags = Dictionary<String, Any>()\neventTags[\"purchasePrice\"] = 64.32\neventTags[\"category\"] = \"shoes\"\neventTags[\"revenue\"] = 6432  // reserved \"revenue\" tag\neventTags[\"value\"] = 4 // reserved \"value\" tag\n\n// Track event with user attributes and event tags\noptimizely?.track(\"my_conversion\",\n                  userId:\"user123\",\n                  attributes:attributes,\n                  eventTags:eventTags)\n                  \n                  ",
      "language": "swift"
    }
  ]
}
[/block]
Event tags are distinct from [user attributes](doc:define-attributes), which should be reserved for user-level targeting and segmentation. Event tags do not affect audiences or the Optimizely results page and do not need to be registered in the Optimizely app.

Event tags are accessible via [raw data export](https://developers.optimizely.com/x/events/export/index.html) in the `event_features` column. Include any event tags you need to reconcile your conversion event data with your data warehouse.
[block:api-header]
{
  "title": "Reserved Tag Keys"
}
[/block]
The table lists the reserved tag keys, which are included in their corresponding fields in the Optimizely event API payload. They're bundled into event tags for your convenience. Use them if you want to benefit from specific reporting features such as revenue metrics or numeric metrics.
[block:parameters]
{
  "data": {
    "h-0": "Tag key",
    "h-1": "Description",
    "0-0": "**revenue**",
    "0-1": "An integer value that is used to track the revenue metric for your experiments, aggregated across all conversion events. Note that:\n* Revenue is recorded in cents; to record a revenue value of $64.32, use `6432`. \n* Add any event you want to track revenue for as a metric in your experiment. \n*Use the overall revenue metric to aggregate multiple metrics tracking separate revenue events. The overall revenue event won't track any revenue unless other metrics in your experiment are tracking an increase or decrease in total revenue; it won't work on its own.",
    "1-0": "**value**",
    "1-1": "A floating point value that is used to track a custom value for your experiments. Use this to pass the value for numeric metrics.\n\n* Note: We don't recommend using total value metrics to track monetary values.\n\nUnlike revenue metrics, which use [fixed-point numbers](https://en.wikipedia.org/wiki/Fixed-point_arithmetic), numeric metrics use [floating-point numbers](https://en.wikipedia.org/wiki/Double-precision_floating-point_format). For example, $72.81 would be submitted as 7281 with revenue, but as 72.81 with value. Due to the dynamic precision of floating-point numbers, aggregations for numeric metrics are [susceptible to rounding](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems). When tracking monetary values, we recommend using the revenue tag to prevent these rounding errors."
  },
  "cols": 2,
  "rows": 2
}
[/block]
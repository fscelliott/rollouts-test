---
title: "Manage bot filtering"
slug: "manage-bot-filtering"
hidden: false
createdAt: "2019-09-11T11:47:40.651Z"
updatedAt: "2019-09-12T18:59:10.707Z"
---
Bot filtering allows you to filter events by sending user agents to Optimizely with your events. Optimizely will compare the list of user agents you send to a list of known bots, and filter all user agents that are on the IAB/ABC blacklist. 
[block:api-header]
{
  "title": "Enable bot filtering"
}
[/block]
Navigate to *Settings* > *Advanced* > *Bot Filtering* in the Optimizely app to turn bot filtering on or off. For more information, see our article [Bot and spider filtering in Optimizely X](https://help.optimizely.com/Account_Settings/Bot_and_spider_filtering_in_Optimizely_X).
[block:api-header]
{
  "title": "Client-side JavaScript events"
}
[/block]
No additional JavaScript code is required to enable bot filtering for events sent from browsers. When you track an event by calling the JavaScript SDK’s Activate, Is Feature Enabled, or Track methods, the SDK automatically includes the user’s user agent in the outbound request. If bot filtering is enabled for your project, Optimizely applies bot filtering automatically.
[block:api-header]
{
  "title": "All other Full Stack events"
}
[/block]
When you track events with an SDK from somewhere other than a web browser, you must pass the user agent to be filtered with your event. You should pass it using the reserved `$opt_user_agent` attribute in the Track, Activate, Is Feature Enabled, and Get Enabled Features functions. If bot filtering is enabled for your project and the user agent is passed in this way, Optimizely applies bot filtering.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "**Enable bot filtering in Optimizely first**. Before implementing the `$opt_user_agent` attribute, navigate to *Settings* > *Advanced* in the Optimizely app to turn bot filtering on or off."
}
[/block]
The example below shows how to pass the `$opt_user_agent` attribute.
[block:code]
{
  "codes": [
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client as the \n * attribute $opt_user_agent\n */\nusing OptimizelySDK.Entity;\n\nvar userAgent = \"this_could_be_a_bot\";\n\nUserAttributes attributes = new UserAttributes\n{\n  { \"device\", \"iphone\" },\n  { \"location\", \"Chicago\" },\n  { \"$opt_user_agent\", userAgent }\n};\n\n// Include user agent when activating an A/B test\noptimizelyClient.Activate(\"some_experiment\", \"some_user\", attributes);\n\n// Include user agent when tracking an event\noptimizelyClient.Track(\"some_conversion_event\", \"some_user\", attributes);\n\n// Include user agent when accessing a feature\noptimizelyClient.IsFeatureEnabled(\"some_feature\", \"some_user\", attributes);\n\n",
      "language": "csharp"
    },
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client as the\n * attribute $opt_user_agent\n */\nString userAgent = \"this_could_be_a_bot\";\n\nMap<String, String> attributes = new HashMap<String, String>();\nattributes.put(\"device\", \"iphone\");\nattributes.put(\"location\", \"Chicago\");\n// Set user agent\nattributes.put(\"$opt_user_agent\", userAgent);\n\n// Include user agent when activating an A/B test\noptimizelyClient.activate(\"some_experiment\", \"some_user\", attributes);\n\n// Include user agent when tracking an event\noptimizelyClient.track(\"some_conversion_event\", \"some_user\", attributes);\n\n// Include user agent when accessing a feature\noptimizelyClient.isFeatureEnabled(\"some_feature\", \"some_user\", attributes);\n\n",
      "language": "java"
    },
    {
      "code": "/**\n * Server-side JavaScript example.\n * No code is required to enable bot filtering for\n * events sent from a browser.\n *\n * Get the user agent and pass it to the Optimizely client as the\n * attribute $opt_user_agent\n */\nvar userAgent = \"this_could_be_a_bot\";\nvar attributes = {\n  device: \"iphone\",\n  location: \"Chicago\",\n  $opt_user_agent: userAgent\n};\n\n// Include user agent when activating an A/B test\noptimizelyClient.activate(\"my_experiment\", userId, attributes);\n\n// Include user agent when tracking an event\noptimizelyClient.track(\"my_conversion\", userId, attributes);\n\n// Include user agent when accessing a feature\noptimizelyClient.isFeatureEnabled(\"price_filter\", userId, attributes);\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client \n *  as the attribute $opt_user_agent\n */\n\nNSString *experimentKey = @\"some_experiment\";\nNSString *userId = @\"some_user\";\nNSDictionary *attributes = @{@\"device\"              : @\"iphone\",\n                             @\"location\"            : @\"Chicago\",\n                             @\"$opt_user_agent\"     : @\"this_could_be_a_bot\"};\n\n// Include user agent when activating an A/B test\nOPTLYVariation *variation = [optimizelyClient activate:experimentKey\n                                                userId:userId\n                                            attributes:attributes];\n\n// Include user agent when tracking an event\nNSString *eventKey = @\"some_conversion_event\";\n[optimizelyClient track:eventKey\n                        userId:userId\n                    attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "/** \n * Get the user agent and pass it to the Optimizely client \n * as the attribute $opt_user_agent\n */\n$userAgent = \"this_could_be_a_bot\";\n\n$attributes = [\n  'device' => 'iphone',\n  'location' => 'Chicago',\n  '$opt_user_agent' => $userAgent\n];\n\n// Include user agent when activating an A/B test\n$optimizelyClient->activate(\"some_experiment\", \"some_user\", $attributes);\n\n// Include user agent when tracking an event\n$optimizelyClient->track(\"some_conversion_event\", \"some_user\", $attributes);\n\n// Include user agent when accessing a feature\n$optimizelyClient->isFeatureEnabled(\"some_feature\", \"some_user\", $attributes);\n\n",
      "language": "php"
    },
    {
      "code": "# Get the user agent and pass it to the Optimizely client \n# as the attribute $opt_user_agent\n\nuser_agent = 'this_could_be_a_bot'\n\nattributes = {\n  'device': 'iphone',\n  'location': 'Chicago',\n  '$opt_user_agent': user_agent\n}\n\n# Include user agent when activating an A/B test\noptimizely_client.activate('some_experiment', 'some_user', attributes)\n\n# Include user agent when tracking an event\noptimizely_client.track('some_conversion_event', 'some_user', attributes)\n\n# Include user agent when accessing a feature\noptimizely_client.is_feature_enabled('some_feature', 'some_user', attributes)\n\n",
      "language": "python"
    },
    {
      "code": "# Get the user agent and pass it to the Optimizely client \n# as the attribute $opt_user_agent\n\nuser_agent = 'this_could_be_a_bot'\n\nattributes = {\n  'device' => 'iphone',\n  'location' => 'Chicago',\n  '$opt_user_agent' => user_agent\n}\n\n# Include user agent when activating an A/B test\noptimizely_client.activate('some_experiment', 'some_user', attributes)\n\n# Include user agent when tracking an event\noptimizely_client.track('some_conversion_event', 'some_user', attributes)\n\n# Include user agent when accessing a feature\noptimizely_client.is_feature_enabled('some_feature', 'some_user', attributes)\n\n",
      "language": "ruby"
    },
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client \n *  as the attribute $opt_user_agent\n */\n\nlet experimentKey = \"some_experiment\"\nlet userId = \"some_user\"\nlet attributes = [\"device\"          : \"iphone\",\n                  \"location\"        : \"Chicago\",\n                  \"$opt_user_agent\" : \"this_could_be_a_bot\"]\n\n// Include user agent when activating an A/B test\noptimizelyClient?.activate(experimentKey, userId, attributes)\n\n// Include user agent when tracking an event\nlet eventKey = 'some_conversion_event'\noptimizelyClient?.track(eventKey, userId, attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
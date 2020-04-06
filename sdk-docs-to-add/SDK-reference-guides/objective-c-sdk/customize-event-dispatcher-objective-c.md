---
title: "Customize event dispatcher"
slug: "customize-event-dispatcher-objective-c"
hidden: false
createdAt: "2019-09-12T15:14:39.490Z"
updatedAt: "2019-09-12T16:00:20.242Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance.
[block:code]
{
  "codes": [
    {
      "code": "CustomEventDispatcher *customEventDispatcher = [[CustomEventDispatcher alloc] init];\n\n/** \n * Initialize your Manager \n *  (settings will propagate to OPTLYClient and Optimizely)\n */\n  OPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n      builder.sdkKey = @\"SDK_KEY_HERE\";\n\t\t\tbuilder.eventDispatcher = customEventDispatcher;\n    }]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let customEventDispatcher: CustomEventDispatcher = CustomEventDispatcher.init()\n\n/**\n * Initialize your Manager \n * (settings will propagate to OPTLYClient and Optimizely)\n */\n let manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.eventDispatcher = customEventDispatcher\n }))\n \n ",
      "language": "swift"
    }
  ]
}
[/block]
The included event handler implementation, [OptimizelySDKEventDispatcher](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKEventDispatcher/OptimizelySDKEventDispatcher/OPTLYEventDispatcher.m), includes queueing and flushing, so will work even if an app is offline. You will need to implement [OPTLYEventDispatcher](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKCore/OptimizelySDKCore/OPTLYEventDispatcherBasic.h) to use your own event handler.
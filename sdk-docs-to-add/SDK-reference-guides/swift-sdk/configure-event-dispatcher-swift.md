---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-swift"
hidden: false
createdAt: "2019-09-12T16:43:14.258Z"
updatedAt: "2020-02-11T22:24:08.537Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance.
[block:code]
{
  "codes": [
    {
      "code": "// create OptimizelyClient with a custom EventDispatcher\nlet customEventDispatcher = CustomEventDispatcher()\n \n// or you can simply adjust parameters of DefaultEventDispatcher to disable batching (timerInterval=0), etc\nlet customEventDispatcher = DefaultEventDispatcher(timerInterval: 0)    \n\noptimizely = OptimizelyClient(sdkKey: \"<Your_SDK_Key>\",\n                              eventDispatcher: customEventDispatcher)\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "// create OptimizelyClient with a custom EventDispatcher\nCustomEventDispatcher *customEventDispatcher = [[CustomEventDispatcher alloc] init];\n     \nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"<Your_SDK_Key>\"\n                       \t\t\tlogger:nil\n                       \t\t\teventDispatcher:customEventDispatcher\n                       \t\t\tuserProfileService:nil\n                         \t\tperiodicDownloadInterval:5*60                       \n                       \t\t\tdefaultLogLevel:OptimizelyLogLevelInfo];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
The event dispatcher should implement a `dispatchEvent` function, which takes in three arguments: `httpVerb`, `url`, and `params`, all of which are created by the internal `EventBuilder` class. In this function, you should send a `POST` request to the given `url` using the `params` as the body of the request (be sure to stringify it to JSON) and `{content-type: 'application/json'}` in the headers.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "If you are using a custom event dispatcher, do not modify the event payload returned from Optimizely. Modifying this payload will alter your results."
}
[/block]
The included event handler implementation for the Swift SDK, [DefaultEventDispatcher](https://github.com/optimizely/swift-sdk/blob/master/Sources/Customization/DefaultEventDispatcher.swift), includes queueing, event batching and flushing. Events will be queued if an app is offline and will be forwarded to the server when the app is online again.
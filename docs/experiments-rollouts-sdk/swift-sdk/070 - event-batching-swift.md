---
title: "Event batching"
slug: "event-batching-swift"
hidden: true
createdAt: "2019-09-12T16:43:05.384Z"
updatedAt: "2019-09-13T13:03:43.221Z"
---
The [Optimizely Full Stack Swift SDK](https://github.com/optimizely/swift-sdk) lets you batch events and includes options to set a maximum batch size, flush interval timeout, and maximum queue size. The benefit of event batching means less network traffic for the same number of Impression and conversion events tracked. 

The event batching functionality works by processing impression events from [Activate](doc:activate-swift) and conversion events from [Track](doc:track-swift) placing them into a queue. The queue is drained when it either reaches its maximum size limit or when the flush interval is triggered.

By default, event batching is enabled in Swift SDK *3.1.0**.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Event batching works with out-of-the-box event dispatchers.\n\nThe event batching process doesn't remove any personally identifiable information (PII) from events. You must still ensure that you aren't sending any unnecessary PII to Optimizely."
}
[/block]

[block:api-header]
{
  "title": "Configure event batching"
}
[/block]
We provide three options to configure event batching: `batchSize`, `timerInterval` and `maxQueueSize`. You can pass in the options during client creation. Events are held in a persistent queue until either:
* The number of events reaches the defined `batchSize`.
* The oldest event has been in the queue for longer than the defined `timerInterval`, which is specified in seconds. The queue is then flushed and all queued events are batched and sent to Optimizely.
* A new datafile revision or project id is received. This occurs only when live datafile updates are enabled.
[block:code]
{
  "codes": [
    {
      "code": "let eventDispatcher = DefaultEventDispatcher(batchSize: 100, \n                                             timerInterval: 30, \n                                             maxQueueSize: 1000)\n\noptimizely = OptimizelyClient(sdkKey: \"<Your_SDK_Key>\",\n                              eventDispatcher: eventDispatcher)\n",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "DefaultEventDispatcher *eventDispatcher = [[DefaultEventDispatcher alloc] \n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tinitWithBatchSize:10 \n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\ttimerInterval:1 \n                \t\t\t\t\t\t\t\t\t\t\t\t\t\t\tmaxQueueSize:1000];\n    \nOptimizelyClient *optimizely = [[OptimizelyClient alloc] \n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tinitWithSdkKey:@\"<Your_SDK_Key>\"\n\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tlogger:nil\n                                      eventDispatcher:eventDispatcher\n                                      userProfileService:nil\n                                      periodicDownloadInterval:nil\n                                      defaultLogLevel:OptimizelyLogLevelInfo];\n",
      "language": "objective-c",
      "name": "Objective-C"
    }
  ]
}
[/block]
The table below defines these options and lists general recommendations for settings. 
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Description",
    "0-0": "**batchSize**",
    "0-1": "The maximum number of events that can be batched together. Once this number of queued events is reached to this size, all queued events are flushed and sent to Optimizely.\n\n**Note**: By setting this value to 0, events aren't batched.",
    "1-1": "The maximum duration in seconds that an event can exist in the queue before being flushed.\n\n**Note**: By setting this value to 0, events aren't batched.",
    "1-0": "**timerInterval**",
    "h-2": "Default",
    "h-3": "",
    "0-2": "10",
    "1-2": "60",
    "0-3": "Based on your organization's requirements.",
    "1-3": "Based on your organization's requirements.",
    "2-0": "**maxQueueSize**",
    "2-1": "The maximum number of events that can be queued. When the queue is full, all new arriving events will be discarded (tail drop).",
    "2-2": "10000"
  },
  "cols": 3,
  "rows": 3
}
[/block]
For more information, see [Initialize SDK](doc:initialize-sdk-swift)
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The maximum payload size is 10 MB. If the resulting batch payload exceeds this limit, requests will be rejected with a 400 response code, `Bad Request Error`."
}
[/block]

[block:api-header]
{
  "title": "Event management with app life cycle"
}
[/block]
When app goes to background, queued events are flushed and the batch timer stops. All the events that cannot be completed with the batching process remain in the queue (persistent).

The batch timer restarts if there are events waiting in the queue when app comes back to foreground.